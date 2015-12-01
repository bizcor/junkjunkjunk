# git-vdiff

A utility program I wrote to help me see git diffs visually.  Prints to stdout vimdiff invocations.  Sometimes I want more than unified diff output in the terminal.  Each invocation compares a file in a commit with its previous version.

If GIT_VDIFF_USER is set in the environment, the program uses it as a search string to identify the author whose commits should be examined.  If GIT_VDIFF_USER is not set, the value of USER in the environment is used.

If ALWAYS_INCLUDE is set in the environment, it is taken to be a list of sha's of commits to be processed irregardless of the commit author.

If GIT_VDIFF_DEBUG is set in the environment, then list all parsed data rather than execute the functionality described above.

## my usage

Examples:

```
$ ( unset ALWAYS_INCLUDE ; unset GIT_VDIFF_DEBUG ; export GIT_VDIFF_USER=bizcor ; ~/projects/bizcor/git-vdiff/bin/git-vdiff.py ~/projects/bizcor/junkjunkjunk/ )
USER => 'bizcor'
ALWAYS_INCLUDE => None
[...snip...]
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo add9617... 632cdcf:a/README.md [M] Tue Dec 1 12:38:14 2015 -0800 "#74" ; git show 632cdcf ) <( echo add9617... c8189db:b/README.md [M] Tue Dec 1 12:38:14 2015 -0800 "#74" ; git show c8189db )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo b99e730... c8189db:a/README.md [M] Tue Dec 1 12:40:52 2015 -0800 "#75" ; git show c8189db ) <( echo b99e730... 3806eda:b/README.md [M] Tue Dec 1 12:40:52 2015 -0800 "#75" ; git show 3806eda )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo dca64f5... 3806eda:a/README.md [M] Tue Dec 1 12:41:45 2015 -0800 "#76" ; git show 3806eda ) <( echo dca64f5... 71b27bf:b/README.md [M] Tue Dec 1 12:41:45 2015 -0800 "#76" ; git show 71b27bf )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 2ac343f... 71b27bf:a/README.md [M] Tue Dec 1 12:42:23 2015 -0800 "#77" ; git show 71b27bf ) <( echo 2ac343f... 5e6f3f5:b/README.md [M] Tue Dec 1 12:42:23 2015 -0800 "#77" ; git show 5e6f3f5 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo c987960... 5e6f3f5:a/README.md [M] Tue Dec 1 12:44:06 2015 -0800 "#78" ; git show 5e6f3f5 ) <( echo c987960... fb29619:b/README.md [M] Tue Dec 1 12:44:06 2015 -0800 "#78" ; git show fb29619 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 0d96528... fb29619:a/README.md [M] Tue Dec 1 12:44:19 2015 -0800 "#79" ; git show fb29619 ) <( echo 0d96528... 94e1971:b/README.md [M] Tue Dec 1 12:44:19 2015 -0800 "#79" ; git show 94e1971 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo e9ed435... 94e1971:a/README.md [M] Tue Dec 1 12:52:30 2015 -0800 "#80" ; git show 94e1971 ) <( echo e9ed435... 4376700:b/README.md [M] Tue Dec 1 12:52:30 2015 -0800 "#80" ; git show 4376700 )
```

Then I edit some-output-file.  In vim, I have the following command defined:

```
:command
    Name        Args Range Complete  Definition
[...]
    Exec        0                    :.w !bash
[...]
```

Then I place the cursor on each line in turn and do:

```
:Exec<return>
```

Here is the debug output:

```
$ ( unset ALWAYS_INCLUDE ; export GIT_VDIFF_DEBUG=1 ; export GIT_VDIFF_USER=bizcor ; ~/projects/bizcor/git-vdiff/bin/git-vdiff.py ~/projects/bizcor/junkjunkjunk/ )
```
