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

###Example:

```
$ ~/projects/bizcor/duplicates/bin/dupscan.py --start-directory venv --field-separator :
manna.local:55f319165970063a80c24d028319341e:16777220:18420306:1:2232:venv/bin/activate
manna.local:164322f70b9d567024e90c2080cef64d:16777220:18420309:1:1258:venv/bin/activate.csh
manna.local:1177f4ed4cb63e5c01276be4d1a8a43a:16777220:18420307:1:2471:venv/bin/activate.fish
manna.local:0f55d499ffc698e5356ec8371f3a9904:16777220:18420308:1:1137:venv/bin/activate_this.py
manna.local:94a7cdf617c44f10f0ee2e7280bab91e:16777220:18419457:1:249:venv/bin/easy_install
manna.local:94a7cdf617c44f10f0ee2e7280bab91e:16777220:18419458:1:249:venv/bin/easy_install-2.7
manna.local:38eb18ad8ad9ac2cfffa15fb13de8537:16777220:18420175:1:221:venv/bin/pip
manna.local:38eb18ad8ad9ac2cfffa15fb13de8537:16777220:18420176:1:221:venv/bin/pip2
manna.local:38eb18ad8ad9ac2cfffa15fb13de8537:16777220:18420177:1:221:venv/bin/pip2.7
manna.local:7c9961756d04568ed2717ff1250b0c90:16777220:18418837:1:25152:venv/bin/python
[...]
```
