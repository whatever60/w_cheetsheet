# Grep

grep, g/re/p, Global regular expression print

Search through your file system for patterns of text

`grep <regular expression> <where you want to search>+`

Return all lines that match the regular expression

`-w`

Only match whole words

`-i`

Case insensitive

`-n`

Also give me line number

`-B`

Specify how many lines that you also want **before** each match.

`-A`

Specify how many lines that you also want **after** each match.

`-C`

Specify how many lines that you also want **before and after / in the context of** each match.

`-r`

Recursive search

Note: now you can pass in directory name (file name is also okay)

`-c`

Only give me number of matches in each file, without content

`-l`

Only give me file names, without content

`-P`

Enable P compatible regular expressions

## Use case

### Search for history

`history | grep "git commit"`

### Search for phone numbers using regular expression

`grep "...-...-...." names.txt`

`grep -P "\d{3}-\d{3}-\d{4}"`

`grep` by default use POSIX regular expression


