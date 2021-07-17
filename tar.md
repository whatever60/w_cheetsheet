# Tar

Compression and archiving.

`tar` stands for 'tape archive'. It is a old command from the 1970s.

## Ref

Youtube[Archiving and Compression on Linux - Basic tar Commands](https://www.youtube.com/watch?v=tSRlNwaUgPQ)


## Basic

```bash
tar zcvf whatever.tar.gz directory_name/
```

Take the `directory_name/` directory and create a new compressed archive file called `whatever.tar.gz`.

`z` for zipping and compression using `gzip`. (you can also use `gzip` seperately). Replace `z` with `I` to use `bzip2` for compression.

`c` for creating new archive.

`v` for verbose. Get one line of output for each file we are archiving.

`f` for file name of the newly created archive file, and the following argument `whatever.tar.gz` is for the flag `f`.

`.tar.gz` is not obligatory, but it is a friendly extension for others to uncompress and unarchive it. Sometimes you'll see `.tgz` as well.

```bash
tar zxvf whatever.tar.gz
```

Decompress and unarchive the archive file and dump everything into the current directory.

`z`, again, for using `gzip` for compression and decompression.

`x` for extract.

```bash
tar tf whatever.tar.gz
```

`t` for list the contents of an archive. This command is recursive, refer to [StackOverflow](https://stackoverflow.com/questions/2700306/listing-the-content-of-a-tar-file-or-a-directory-only-down-to-some-level) if you only want contents down to some depth. (HINT: no trivial solution)

## Tar bomb

```bash
tar zcf ../tarbomb.tar.gz .
```

Note: `tar zcf tarbomb.tar.gz .` will raise an error `tar: .: file changed as we read it`.

This will blow up your current directory when you unarchive it.

So always archive outside of a stand-alone directory.

