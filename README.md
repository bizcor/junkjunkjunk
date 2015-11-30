# snap

snap is a shell program that creates timestamped copies of files.  By default, the timestamp in the file name of each copy represents the mtime of the original file.

## Why snap?

What are the advantages of using snap?  Why not just do this?

```$ cp myfile myfile.bak```

Many do.  But here are some advantages of using snap:

* snap preserves file ownership(*)
* snap preserves file mtime(*)

  [*] given sufficient privilege
