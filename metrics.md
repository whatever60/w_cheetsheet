# Metrics for Clustering Algorithms in Scikit-Learn

## What is in common

All these algorithms take two set of label assignments (of `int` type and of the same length) as input, and output a scaler value.

## MI - Mutual information

$$
M I(U, V)=\sum_{i=1}^{|U|} \sum_{j=1}^{|V|} \frac{\left|U_{i} \cap V_{j}\right|}{N} \log \frac{N\left|U_{i} \cap V_{j}\right|}{\left|U_{i}\right|\left|V_{j}\right|}
$$

Non-negative.

Symmetric.

## NMI - Normalized mutual information

$$
\operatorname{NMI}(A ; B)=\frac{\mathrm{I}(A ; B)}{\sqrt{\mathrm{H}(A) \mathrm{H}(B)}}
$$

Mutual information normalized by some mean of entropy.

The above form is geometric mean. Min/Max/Arithmetic mean are other avaliable options in sklearn.

`[0, 1]`, `0` means no mutual information. `1` means perfect correlation.

Not adjusted for chance.

Symmetric.

## AMI - Adjusted mutual information

$$
\operatorname{AMI}(U, V) = \frac{\operatorname{MI}(U, V) - \mathbb E[\operatorname{MI}(U, V)]}{\operatorname{avg}(\mathrm H(U), \mathrm H(V)) - \mathbb E[\operatorname{MI}(U, V)]}
$$

> It accounts for the fact that the MI is generally higher for two clusterings with a larger number of clusters.

`1` means perfect correlation. About `0` for Independent assignments, and could be less than `0`.

Symmetric.

Slower.
