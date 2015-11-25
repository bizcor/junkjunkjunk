# duplicates

Python code to detect duplicate files.  My definition of duplicates is two files that do not share an inode, but have the same md5 checksum.
Duplicates can be detected within filesystems or across filesystems, whether or not they're on the same host.

## Detecting duplicates

**dupscan.py** scans a filesystem tree rooted at the given directory, or rooted at the current working directory if no directory is specified.  It traverses the tree processing non-symlink regular files.  For each such file it encounters, it writes to stdout a line of data that is formatted as follows.  I show a colon as a field delimiter here, but the default delimiter is an ascii nul character (\0).

*HOSTNAME:MD5_CHECKSUM:FILESYSTEM_DEVICE_NUMBER:INODE_NUMBER:SIZE_IN_BYTES:PATH*


