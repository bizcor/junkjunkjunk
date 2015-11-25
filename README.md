# duplicates

Python code to detect duplicate files.  My definition of duplicates is two files that do not share an inode, but have the same md5 checksum.
Duplicates can be detected within filesystems or across filesystems, on the same host, or across hosts.  I wrote the code to identify multiple locations of large data files so I could delete the duplicates and reclaim filesystem space.

The idea is to scan one or more filesystem trees and then parse the data, reporting on which files are duplicates, as defined above.

## Detecting duplicates

**dupscan.py** scans a filesystem tree rooted at the given directory, or rooted at the current working directory if no directory is specified.  It traverses the tree processing non-symlink regular files.  For each such file it encounters, it writes to stdout a line of data that is formatted as follows.  I show a colon as a field delimiter here, but the default delimiter is an ascii nul character (\0).

*HOSTNAME:MD5_CHECKSUM:FILESYSTEM_DEVICE_NUMBER:INODE_NUMBER:SIZE_IN_BYTES:PATH*


