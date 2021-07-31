## torchmetrics

Ref: [Overview — PyTorch-Metrics 0.4.1 documentation (torchmetrics.readthedocs.io)](https://torchmetrics.readthedocs.io/en/latest/pages/overview.html)

- Functional or modular

  Functional和`LightningModule`耦合更松，函数式更简单轻量，适合即取即用。

  Module和`LightningModule`之间有所耦合，更方便构建系统，而且可以负责DDP的sync和batch间的汇总。

- 分布式

  - 如果用DP，metric初始化时须设置`dist_sync_on_step=True`。因为DP在一次forward后会清理掉各个进程计算的metric，所以必须在被清理之前完成sync。此外，在和`LightningModule`搭配使用时，metric计算应在`training_step_end`完成。

  - 如果用DDP，需要注意DDP会在drop_last=False时从所有数据中随机选取样本填满最后一个batch（即将样本数量填充至能被`batch_size * num_gpus`整除），保证每个GPU负载相同，所以严格说计算出的指标值是不准确的。在训练和验证中这个问题不大，可以不做处理，但测试时尽量只用一个GPU，或者用DDP + `join context`。

- 可微性

  只要计算metric的每步操作都可微，那相应的metric（module和functional形式）就都可微。但module中的缓存是detach过的，所以只有当前step计算出来的量可微，缓存的和积累的统计量不可微。

- 其他注意事项

  - Metric module会将所有跟踪所有见过的数据，所以train、validate、test的module不能混用，要分别初始化各自的module。

  - 默认情况下metric module不会被存到state_dict中。但这个可以通过`persistent`方法设置。

  - 不是所有metrics都支持半精度计算。

  - Metrics module的操作符被重载为返回一个新的metrics。

## Distributed training

- 几点要素

  - Lightning会负责构建distributed sampler，但会默认将train dataloader的shuffle设置为True，将val/test dataloader的shuffle设为False，将drop_last设为PyTorch中的默认值（目前是`False`，估计以后也不会变）。你可以在trainer中指定`replace_sampler_ddp=False`，然后自己配置distributed sampler。此外，Lightning不会为iterable dataset自动配置sampler。

  - GPU训练时Lightning默认使用`nccl`后端（不用`gloo`）。

- 各种分布式方式

  - DP，Data Parallel，单机多卡。已经是Legacy了，Lightning的支持也不好，不要使用。将一个batch分散到每个GPU（注意区别于每个sampler单独负责一个GPU）。坏处是每次前向传播中，各个进程的replica的状态不会被保存，会被清理掉，所以在每个step中assign state可能会报错。
  - DDP，DistributedDataParallel，多机多卡。
    - 每个GPU始终只能看到整个dataset的一个子集。训练过程中每个GPU分别初始化一次模型，并行执行完整的前传和反传，然后梯度同步，在所有进程间平均，最后每个进行的优化器更新权重。
    - DDP又分为基于python脚本的和基于spawn的，基于spawn的DDP是默认配置，但它有几个弊端：它是在subprocess中训练模型的，main process中的模型权重不会更新，所以checkpoint的存取需要另外进行；dataloader的worker数量较大时会成为bottleneck，速度极慢，这是一个源自PyTorch的局限；要求训练涉及到的所有对象都是picklable的。基于脚本的要快得多，但在少数情况下只能用DP或者DDP spawn，包括Jupyter Notebook、COLAB、KAGGLE以及类似的环境，还有就是在nested script without a root package的时候。
    - 如果用了`seed_everything`，每个sampler会设置相同的seed。
  - DDP2，DistributedDataParallel2，多机多卡，单个机器上的卡之间用DP，机器之间用DDP。这种设置有时候有好处，比如自监督中需要较多负样本的时候，DP就可以使用整个机器上所有GPU的batch计算NCE Loss等，因为the full batch is visible to all GPUs on the node。
  - Horovod，第三方分布式框架，适用于许多深度学习框架，设计上遵循MPI标准。
  - TPU。

- 注意事项
 
  - 验证和测试过程中务必同步每个进程的logging，不然出大问题，具体操作是log的时候加上`sync_dist=True`。但如果用的是`torchmetrics`中实现的metrics（或者符合`Metrics API`的custom metrics）就不用管了，这个会自动同步。
