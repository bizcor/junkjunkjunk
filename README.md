# snap

snap is a shell program that creates timestamped copies of regular files.  By default, the timestamp in the file name of each copy represents the mtime of the original file.

## Why snap?

What are the advantages of using snap?  Why not just do this?

```$ cp myfile myfile.bak```

Many do.  But here are some advantages of using snap:

* snap preserves file ownership, permissions, and modification and access times[*].  `cp -p` does this, you say.  True, but snap has additional advantages...

* The basenames of the copied files each contain a timestamp.  By default this is the mtime of the original file.  Having the mtime in the timestamp easily and quickly lets you identify the age of the original file.  It also allows you to use snap to copy multiple versions of a file.  Isn't this what source control is for, you might ask.  Yes.  snap is not intended to be a replacement for source control.  It is intended to be used for files you don't currently, or don't intend to put under source control.  You can stop reading now if you source control absolutely every file you modify. :-D

* The timestamps include time zone offsets!  I wish all timestamps had time zone offsets.  &lt;&#42;sigh*>

* If you would rather the timestamp of each copy represent the current time, there is an option available to do that.  But this is not the default.

* By default, snap saves each file copied in the directory of its original file, not the current directory

* If you don't want each copy saved in the directory of its original file, you can specify a directory into which all files are saved for that invocation

[*] given sufficient privilege

## Examples

```
     # includes a non-existent file, a device file, a root directory file, and a non-readble file
$ snap zzzzz /dev/null /installer.failurerequests ~/tmp/noread ~/bin/gpgedit gmvault.log ; echo status=$?
snap: skipping 'zzzzz': can't stat
snap: skipping '/dev/null': not regular file
snap: skipping '/installer.failurerequests': cannot write to directory '/'
snap: skipping '/Users/blarf/tmp/noread': no read permission
/Users/blarf/bin/gpgedit.20140928.154916-0800
./gmvault.log.20141231.211200-0800
status=1

     # specify directory for copies
$ snap -d /var/tmp ~/bin/gpgedit gmvault.log ; echo status=$?
/var/tmp/gpgedit.20140928.154916-0800
/var/tmp/gmvault.log.20141231.211200-0800
status=0

     # indicate that current time should be used rather than mtimes of original files
$ snap -nd /var/tmp ~/bin/gpgedit gmvault.log ; echo status=$?
/var/tmp/gpgedit.20151130.125757-0800
/var/tmp/gmvault.log.20151130.125757-0800
status=0

     # use different time zone
$ (export TZ=GMT ; snap -d /var/tmp ~/bin/gpgedit gmvault.log ; echo status=$?)
/var/tmp/gpgedit.20140928.224916+0000
/var/tmp/gmvault.log.20150101.051200+0000
status=0
```
