# Shell Commands

```bash
adduser
apropos
awk
cal
cat
cd
chmod
chown
cp
curl
cut
date
df
du
echo
exit
export
file
find
grep
head
help
history
htop
id
info
kill
killall
less
ln
ls
lsb_release
man
mkdir
more
mount
mv
ps
pwd
rm
rmdir
sed
seq
shuf
sleep
sort
source
su
sudo
tail
time
top
touch
umount
uniq
useradd
users
wc
wget
whatis
whereis
which
whoami
xargs
yes
zless
```

```bash
md5sum
gzip
tar
ungzip
```

```bash
df
du
chmod
chown
mount
umount
```

```bash
cp
cut
ls
mkdir
mv
pwd
rm
rmdir
```


```bash
awk
cut
find
grep
sed
xargs
```

```bash
info
help
man
whatis
whereis
which
```

### `shuf`

Shuffle.

```bash
> shuf -n 2 -i 1-10 --repeat  # same as `np.random.choice(np.arange(1, 11), size=(2,), replace=True)`
3
4
```

```bash
shuf file1.txt -o file2.txt  # output to file
```

### `sort`

Sort.

```bash
sort -r file.txt  # sort in reverse alphabetic order
```

```bash
sort -t : -k 3n /etc/passwd  # sort the file by the third column (colon separated)
```

```
-f  Ignore case (fold lowercase to upper case)
-i  Consider only printable characters (and they won't appear in the output as well)
-n  Numeric sort
-o  Output to file
-r  Sort in reverse order
-R  Shuffle, but group identical keys
-u  Only return unique lines, no duplication
```

### `uniq`

Merging adjacent matching lines. Often used together with `sort`.

```
-c  Also return count at the beginning of each line
-d  Only return duplicate lines
-u  Only return unique lines
```

```bash
sort file.txt | uniq -c | sort -nr  # sort by frequency
```

## Ref

[Three Incredibly Useful Command Line Tools (shuf, sort, uniq) - YouTube](https://www.youtube.com/watch?v=31hGtM4s5JE)
