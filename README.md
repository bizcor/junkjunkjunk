# duplicates

Python code to detect duplicate files.  My definition of duplicates is two files that do not share an inode, but have the same md5 checksum.
Duplicates can be detected within filesystems or across filesystems, on the same host, or across hosts.  I wrote the code to identify multiple locations of large data files so I could delete the duplicates and reclaim filesystem space.

The idea is to scan one or more filesystem trees and then parse the data, reporting on which files are duplicates, as defined above.

## Detecting duplicates

**dupscan.py** scans a filesystem tree rooted at the given directory, or rooted at the current working directory if no directory is specified.  It traverses the tree processing regular files.  For each such file it encounters, it writes to stdout a line of data that is formatted as follows.  I show the string ' : ' as a field delimiter here for readability, but the default delimiter is an ascii nul character (\0).

*HOSTNAME : MD5_CHECKSUM : FILESYSTEM_DEVICE_NUMBER : INODE_NUMBER : SIZE_IN_BYTES : PATH*

```
usage: dupscan.py [-h] [-d START_DIRECTORY] [-s FIELD_SEPARATOR]

scan files in a tree and print to stdout a line of information about each regular file

optional arguments:
  -h, --help            show this help message and exit
  -d START_DIRECTORY, --start-directory START_DIRECTORY
                        Specify the root of the filesystem tree to be
                        processed. The default is the current directory.
  -s FIELD_SEPARATOR, --field-separator FIELD_SEPARATOR
                        Specify the string to use as a field separator in
                        output. The default is the ascii nul character.
```

**parsedups.py** takes input from stdin or files or both.  It expects each line to be of the format of lines created by dupscan.py.  It reads the data for all input sources into memory and outputs information on any duplicate files it detects.

```
usage: parsedups.py [-h] [--file FILE] [--field-separator FIELD_SEPARATOR]
                    [--size SIZE] [--list-fields]

scan files in a tree and print a line of information about each regular file

optional arguments:
  -h, --help            show this help message and exit
  --file FILE, -f FILE  File(s) from which to read data. The default is to
                        read from stdin. To mix stdin and files, specify stdin
                        as a file called "__stdin__"
  --field-separator FIELD_SEPARATOR, -s FIELD_SEPARATOR
                        Specify the string to use as a field separator in
                        output. The default is the ascii nul character.
  --size SIZE, -S SIZE  Specify the size (in bytes) a file must be to be
                        considered for processing. The default size is 0.
  --list-fields, -l     Print data for all files whether or not they're
                        duplicates
```
