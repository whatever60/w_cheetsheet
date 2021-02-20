# Find

Scan through our file system to find files and directories that meet a certain criteria

## Basic use case

`find .`

List all files and directories recursively under current directory.

`find . -maxdepth 1`

Only search down 1 directory.

## Search by type

`find . -type d`

List only directories, no files.

`find . -type f`

List only files, no directories.

## Search by name

`find . -type f -name "test*"`

Match file name, with wildcard.

`find . -type f -iname "test*"`

Same as above, case insensitive.

## Search by timestamp

`find . -type f -mmin -10`

Find files modified less than 10 minutes ago.

`find . -type f -mmin 10`

Find fils modified 9~10 minutes before now.

`find . -type f -mmin +1 -mmin -5`

Combine multiple of these conditions.

Find files modified more than 1 minute ago, but less than 5 minutes ago.

`-amin` / `-cmin`

Accessed time and changed time.

Note: modify means file content is changed, change means either file content or metadata such as permission and owner is changed.

`-mtime` / `-atime` / `-ctime`

Time in days.

Note: the difference in days is calculated such that any fractional part is ignored. Therefore, `-mtime +1` means modified at least 48 hours ago.

## Search by file size

`find . -size +5M`

Note: `k` for kilobyte and `G` for gigabyte.

`find . -empty`

Search for empty files.

## Search by permissions

`find . -perm 777`

## Execute command on searching results

`find . -exec chown wusuowei:wusuowei {} +`

Batch modifying file owners.

Note: `{}` is placeholder for file and directory name, and `+` or `\;` means end of the command.

`find . -type d -exec chmod 775 {} +`

`find . -type f -exec chmod 664 {} +`

Batch changing file permissions.


