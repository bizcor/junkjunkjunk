# git-vdiff

A utility program I wrote to help me see git diffs visually.  Prints to stdout vimdiff invocations.  Each invocation compares a file in a commit with its previous version.

If GIT_VDIFF_USER is set in the environment, the program uses it as a search string to identify the author whose commits should be examined.  If GIT_VDIFF_USER is not set, the value of USER in the environment is used.

If ALWAYS_INCLUDE is set in the environment, it is taken to be a list of sha's of commits to be processed irregardless of the commit author.

If GIT_VDIFF_DEBUG is set in the environment, then list all parsed data rather than execute the functionality described above.

## my usage

Example:

```
$ export GIT_VDIFF_USER=bizcor ; git-vdiff | grep some-specific-file > some-output-file
```

Then I edit some-output-file.  In vim, I have the following command defined:

```
:command
    Name        Args Range Complete  Definition
[...]
    Exec        0                    :.w !bash
[...]
```
