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
$ dupscan.py --start-directory venv --field-separator :
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
[...output truncated for readability in this README.]

$ dupscan.py -d venv | parsedups.py --list | wc -l
parsedups.py: args => {'files': [], 'list_fields': True, 'size': 0, 'field_separator': '\x00'}
     698
```

If I indicate it should list files of size 100,000 bytes or greater:

```
$ dupscan.py -d venv | parsedups.py --list --size 100000
parsedups.py: args => {'files': [], 'list_fields': True, 'size': 100000, 'field_separator': '\x00'}
manna.local  fb0e405e7b3311dd6a46cf5918d41bf1      133353    18419337  venv/lib/python2.7/site-packages/pkg_resources/__init__.pyc
manna.local  4ea2a802c8f2188dcff76e40c0ec77e2      106466    18419911  venv/lib/python2.7/site-packages/pip/_vendor/pkg_resources/__init__.py
manna.local  3578983e05e8367767122054b8e40ad1      113855    18419961  venv/lib/python2.7/site-packages/pip/_vendor/requests/packages/chardet/big5freq.pyc
manna.local  eb9de34540cc21712b38f555ad3415bb      134764    18419912  venv/lib/python2.7/site-packages/pip/_vendor/pkg_resources/__init__.pyc
manna.local  aeaceddba964d9486b92c111ee77c2b0      106670    18419336  venv/lib/python2.7/site-packages/pkg_resources/__init__.py
manna.local  ffb367a1b27f407fe02ecd0a8cbefc35      117347    18419810  venv/lib/python2.7/site-packages/pip/_vendor/html5lib/html5parser.py
manna.local  6f58f7b3d7b46120dfa9839a15e2ef4e      136381    18419811  venv/lib/python2.7/site-packages/pip/_vendor/html5lib/html5parser.pyc
manna.local  cc3d2cd6b035dc31b2614c1df204848e      308434    18419933  venv/lib/python2.7/site-packages/pip/_vendor/requests/cacert.pem
```
