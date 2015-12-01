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
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo c7d97c4... 0000000:a/README.md [A] Wed Nov 25 10:14:51 2015 -0800 "#1" ; git show 0000000 ) <( echo c7d97c4... e79ca41:b/README.md [A] Wed Nov 25 10:14:51 2015 -0800 "#1" ; git show e79ca41 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 9e7aec8... e79ca41:a/README.md [M] Wed Nov 25 10:16:00 2015 -0800 "#2" ; git show e79ca41 ) <( echo 9e7aec8... fe71ace:b/README.md [M] Wed Nov 25 10:16:00 2015 -0800 "#2" ; git show fe71ace )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 8c0d4de... fe71ace:a/README.md [M] Wed Nov 25 10:17:22 2015 -0800 "#3" ; git show fe71ace ) <( echo 8c0d4de... 43111a8:b/README.md [M] Wed Nov 25 10:17:22 2015 -0800 "#3" ; git show 43111a8 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 6e30423... 43111a8:a/README.md [M] Wed Nov 25 10:17:36 2015 -0800 "#4" ; git show 43111a8 ) <( echo 6e30423... 0679b8d:b/README.md [M] Wed Nov 25 10:17:36 2015 -0800 "#4" ; git show 0679b8d )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 6df9f05... 0679b8d:a/README.md [M] Wed Nov 25 10:18:20 2015 -0800 "#5" ; git show 0679b8d ) <( echo 6df9f05... a5c0f02:b/README.md [M] Wed Nov 25 10:18:20 2015 -0800 "#5" ; git show a5c0f02 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo b1a64ec... a5c0f02:a/README.md [M] Wed Nov 25 10:18:42 2015 -0800 "#6" ; git show a5c0f02 ) <( echo b1a64ec... 04afa32:b/README.md [M] Wed Nov 25 10:18:42 2015 -0800 "#6" ; git show 04afa32 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d61e5ad... 04afa32:a/README.md [M] Wed Nov 25 10:19:28 2015 -0800 "#7" ; git show 04afa32 ) <( echo d61e5ad... 991cb28:b/README.md [M] Wed Nov 25 10:19:28 2015 -0800 "#7" ; git show 991cb28 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 10969ce... 991cb28:a/README.md [M] Wed Nov 25 10:19:51 2015 -0800 "#8" ; git show 991cb28 ) <( echo 10969ce... 3776a21:b/README.md [M] Wed Nov 25 10:19:51 2015 -0800 "#8" ; git show 3776a21 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo ccc3d1a... 3776a21:a/README.md [M] Wed Nov 25 10:20:34 2015 -0800 "#9" ; git show 3776a21 ) <( echo ccc3d1a... 1c919d2:b/README.md [M] Wed Nov 25 10:20:34 2015 -0800 "#9" ; git show 1c919d2 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d3ece34... 1c919d2:a/README.md [M] Wed Nov 25 10:21:33 2015 -0800 "#10" ; git show 1c919d2 ) <( echo d3ece34... c5691e2:b/README.md [M] Wed Nov 25 10:21:33 2015 -0800 "#10" ; git show c5691e2 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 079f3fb... c5691e2:a/README.md [M] Wed Nov 25 10:23:00 2015 -0800 "#11" ; git show c5691e2 ) <( echo 079f3fb... 5e34ce8:b/README.md [M] Wed Nov 25 10:23:00 2015 -0800 "#11" ; git show 5e34ce8 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 1f7252d... 5e34ce8:a/README.md [M] Wed Nov 25 10:25:23 2015 -0800 "#12" ; git show 5e34ce8 ) <( echo 1f7252d... dee649d:b/README.md [M] Wed Nov 25 10:25:23 2015 -0800 "#12" ; git show dee649d )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 7b3a813... dee649d:a/README.md [M] Wed Nov 25 10:26:06 2015 -0800 "#13" ; git show dee649d ) <( echo 7b3a813... 7b204f9:b/README.md [M] Wed Nov 25 10:26:06 2015 -0800 "#13" ; git show 7b204f9 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 0a29f33... 7b204f9:a/README.md [M] Wed Nov 25 10:26:59 2015 -0800 "#14" ; git show 7b204f9 ) <( echo 0a29f33... bc04241:b/README.md [M] Wed Nov 25 10:26:59 2015 -0800 "#14" ; git show bc04241 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo bb381db... bc04241:a/README.md [M] Wed Nov 25 10:27:28 2015 -0800 "#15" ; git show bc04241 ) <( echo bb381db... 5dfc387:b/README.md [M] Wed Nov 25 10:27:28 2015 -0800 "#15" ; git show 5dfc387 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 4f3460a... 5dfc387:a/README.md [M] Wed Nov 25 10:28:09 2015 -0800 "#16" ; git show 5dfc387 ) <( echo 4f3460a... 25d7def:b/README.md [M] Wed Nov 25 10:28:09 2015 -0800 "#16" ; git show 25d7def )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 9d8eeda... 25d7def:a/README.md [M] Wed Nov 25 10:53:33 2015 -0800 "#17" ; git show 25d7def ) <( echo 9d8eeda... e24a4d6:b/README.md [M] Wed Nov 25 10:53:33 2015 -0800 "#17" ; git show e24a4d6 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 6c535c0... e24a4d6:a/README.md [M] Wed Nov 25 11:12:05 2015 -0800 "#18" ; git show e24a4d6 ) <( echo 6c535c0... 4527db8:b/README.md [M] Wed Nov 25 11:12:05 2015 -0800 "#18" ; git show 4527db8 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d8706ef... 4527db8:a/README.md [M] Wed Nov 25 11:14:01 2015 -0800 "#19" ; git show 4527db8 ) <( echo d8706ef... 1bf880b:b/README.md [M] Wed Nov 25 11:14:01 2015 -0800 "#19" ; git show 1bf880b )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 92ec71a... 1bf880b:a/README.md [M] Wed Nov 25 11:22:40 2015 -0800 "#20" ; git show 1bf880b ) <( echo 92ec71a... e47a099:b/README.md [M] Wed Nov 25 11:22:40 2015 -0800 "#20" ; git show e47a099 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo adbf1ca... e47a099:a/README.md [M] Wed Nov 25 11:41:59 2015 -0800 "#21" ; git show e47a099 ) <( echo adbf1ca... 043e67f:b/README.md [M] Wed Nov 25 11:41:59 2015 -0800 "#21" ; git show 043e67f )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo e917a5f... 043e67f:a/README.md [M] Wed Nov 25 11:42:33 2015 -0800 "#22" ; git show 043e67f ) <( echo e917a5f... 0dffef0:b/README.md [M] Wed Nov 25 11:42:33 2015 -0800 "#22" ; git show 0dffef0 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 8f9672a... 0dffef0:a/README.md [M] Wed Nov 25 11:44:08 2015 -0800 "#23" ; git show 0dffef0 ) <( echo 8f9672a... ffe5979:b/README.md [M] Wed Nov 25 11:44:08 2015 -0800 "#23" ; git show ffe5979 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 4f6a877... ffe5979:a/README.md [M] Wed Nov 25 12:13:42 2015 -0800 "#24" ; git show ffe5979 ) <( echo 4f6a877... 22a5d25:b/README.md [M] Wed Nov 25 12:13:42 2015 -0800 "#24" ; git show 22a5d25 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo a7d9a15... 22a5d25:a/README.md [M] Wed Nov 25 12:14:58 2015 -0800 "#25" ; git show 22a5d25 ) <( echo a7d9a15... 46932b0:b/README.md [M] Wed Nov 25 12:14:58 2015 -0800 "#25" ; git show 46932b0 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo b25d03c... 46932b0:a/README.md [M] Wed Nov 25 12:15:14 2015 -0800 "#26" ; git show 46932b0 ) <( echo b25d03c... 8fe4993:b/README.md [M] Wed Nov 25 12:15:14 2015 -0800 "#26" ; git show 8fe4993 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 60b06d6... 8fe4993:a/README.md [M] Wed Nov 25 12:16:30 2015 -0800 "#27" ; git show 8fe4993 ) <( echo 60b06d6... 1a66ac6:b/README.md [M] Wed Nov 25 12:16:30 2015 -0800 "#27" ; git show 1a66ac6 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 7dd547b... 1a66ac6:a/README.md [M] Wed Nov 25 12:17:09 2015 -0800 "#28" ; git show 1a66ac6 ) <( echo 7dd547b... 5e2029c:b/README.md [M] Wed Nov 25 12:17:09 2015 -0800 "#28" ; git show 5e2029c )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 65b561a... 5e2029c:a/README.md [M] Wed Nov 25 12:20:33 2015 -0800 "#29" ; git show 5e2029c ) <( echo 65b561a... b069723:b/README.md [M] Wed Nov 25 12:20:33 2015 -0800 "#29" ; git show b069723 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 1003008... b069723:a/README.md [M] Wed Nov 25 12:34:42 2015 -0800 "#30" ; git show b069723 ) <( echo 1003008... 0901055:b/README.md [M] Wed Nov 25 12:34:42 2015 -0800 "#30" ; git show 0901055 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 9f34b44... 0901055:a/README.md [M] Wed Nov 25 12:36:10 2015 -0800 "#31" ; git show 0901055 ) <( echo 9f34b44... 011b98f:b/README.md [M] Wed Nov 25 12:36:10 2015 -0800 "#31" ; git show 011b98f )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo bd15c48... 011b98f:a/README.md [M] Wed Nov 25 12:36:33 2015 -0800 "#32" ; git show 011b98f ) <( echo bd15c48... 841d356:b/README.md [M] Wed Nov 25 12:36:33 2015 -0800 "#32" ; git show 841d356 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 1de0aea... 841d356:a/README.md [M] Wed Nov 25 12:37:03 2015 -0800 "#33" ; git show 841d356 ) <( echo 1de0aea... 0c4c413:b/README.md [M] Wed Nov 25 12:37:03 2015 -0800 "#33" ; git show 0c4c413 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 549c068... 0c4c413:a/README.md [M] Wed Nov 25 12:40:26 2015 -0800 "#34" ; git show 0c4c413 ) <( echo 549c068... c22a5e4:b/README.md [M] Wed Nov 25 12:40:26 2015 -0800 "#34" ; git show c22a5e4 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 3f3c3ab... c22a5e4:a/README.md [M] Wed Nov 25 12:42:12 2015 -0800 "#35" ; git show c22a5e4 ) <( echo 3f3c3ab... f6d812f:b/README.md [M] Wed Nov 25 12:42:12 2015 -0800 "#35" ; git show f6d812f )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 0ac6e52... f6d812f:a/README.md [M] Wed Nov 25 12:44:20 2015 -0800 "#36" ; git show f6d812f ) <( echo 0ac6e52... 9abf058:b/README.md [M] Wed Nov 25 12:44:20 2015 -0800 "#36" ; git show 9abf058 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 1e0d0f9... 9abf058:a/README.md [M] Wed Nov 25 12:46:00 2015 -0800 "#37" ; git show 9abf058 ) <( echo 1e0d0f9... 3a8bf08:b/README.md [M] Wed Nov 25 12:46:00 2015 -0800 "#37" ; git show 3a8bf08 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo cd42e90... 3a8bf08:a/README.md [M] Wed Nov 25 12:47:05 2015 -0800 "#38" ; git show 3a8bf08 ) <( echo cd42e90... 4069d90:b/README.md [M] Wed Nov 25 12:47:05 2015 -0800 "#38" ; git show 4069d90 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 6e80a48... 4069d90:a/README.md [M] Wed Nov 25 12:59:12 2015 -0800 "#39" ; git show 4069d90 ) <( echo 6e80a48... 505db53:b/README.md [M] Wed Nov 25 12:59:12 2015 -0800 "#39" ; git show 505db53 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 2f93c61... 505db53:a/README.md [M] Wed Nov 25 16:17:06 2015 -0800 "#40" ; git show 505db53 ) <( echo 2f93c61... 59fb486:b/README.md [M] Wed Nov 25 16:17:06 2015 -0800 "#40" ; git show 59fb486 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 2cf6c95... 59fb486:a/README.md [M] Mon Nov 30 12:09:04 2015 -0800 "#41" ; git show 59fb486 ) <( echo 2cf6c95... b8b4695:b/README.md [M] Mon Nov 30 12:09:04 2015 -0800 "#41" ; git show b8b4695 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 3864560... b8b4695:a/README.md [M] Mon Nov 30 12:10:14 2015 -0800 "#42" ; git show b8b4695 ) <( echo 3864560... 4451178:b/README.md [M] Mon Nov 30 12:10:14 2015 -0800 "#42" ; git show 4451178 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d27709a... 4451178:a/README.md [M] Mon Nov 30 12:12:49 2015 -0800 "#43" ; git show 4451178 ) <( echo d27709a... 5400186:b/README.md [M] Mon Nov 30 12:12:49 2015 -0800 "#43" ; git show 5400186 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo aede92a... 5400186:a/README.md [M] Mon Nov 30 12:14:05 2015 -0800 "#44" ; git show 5400186 ) <( echo aede92a... 720e75e:b/README.md [M] Mon Nov 30 12:14:05 2015 -0800 "#44" ; git show 720e75e )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo a9ceb03... 720e75e:a/README.md [M] Mon Nov 30 12:14:15 2015 -0800 "#45" ; git show 720e75e ) <( echo a9ceb03... 284340f:b/README.md [M] Mon Nov 30 12:14:15 2015 -0800 "#45" ; git show 284340f )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 2f8d52e... 284340f:a/README.md [M] Mon Nov 30 12:14:32 2015 -0800 "#46" ; git show 284340f ) <( echo 2f8d52e... fe81bc3:b/README.md [M] Mon Nov 30 12:14:32 2015 -0800 "#46" ; git show fe81bc3 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo b73aeaa... fe81bc3:a/README.md [M] Mon Nov 30 12:17:09 2015 -0800 "#47" ; git show fe81bc3 ) <( echo b73aeaa... 22953ab:b/README.md [M] Mon Nov 30 12:17:09 2015 -0800 "#47" ; git show 22953ab )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo fedad71... 22953ab:a/README.md [M] Mon Nov 30 12:17:32 2015 -0800 "#48" ; git show 22953ab ) <( echo fedad71... 3b2f184:b/README.md [M] Mon Nov 30 12:17:32 2015 -0800 "#48" ; git show 3b2f184 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 49473ee... 3b2f184:a/README.md [M] Mon Nov 30 12:18:12 2015 -0800 "#49" ; git show 3b2f184 ) <( echo 49473ee... c9dcd61:b/README.md [M] Mon Nov 30 12:18:12 2015 -0800 "#49" ; git show c9dcd61 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 88225c5... c9dcd61:a/README.md [M] Mon Nov 30 12:18:32 2015 -0800 "#50" ; git show c9dcd61 ) <( echo 88225c5... d4be760:b/README.md [M] Mon Nov 30 12:18:32 2015 -0800 "#50" ; git show d4be760 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo c6424dd... d4be760:a/README.md [M] Mon Nov 30 12:18:47 2015 -0800 "#51" ; git show d4be760 ) <( echo c6424dd... dbc0da5:b/README.md [M] Mon Nov 30 12:18:47 2015 -0800 "#51" ; git show dbc0da5 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo cd26103... dbc0da5:a/README.md [M] Mon Nov 30 12:19:02 2015 -0800 "#52" ; git show dbc0da5 ) <( echo cd26103... 18e1616:b/README.md [M] Mon Nov 30 12:19:02 2015 -0800 "#52" ; git show 18e1616 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 737a7d8... 18e1616:a/README.md [M] Mon Nov 30 12:21:39 2015 -0800 "#53" ; git show 18e1616 ) <( echo 737a7d8... 68a9108:b/README.md [M] Mon Nov 30 12:21:39 2015 -0800 "#53" ; git show 68a9108 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo cf96567... 68a9108:a/README.md [M] Mon Nov 30 12:23:36 2015 -0800 "#54" ; git show 68a9108 ) <( echo cf96567... cb409fc:b/README.md [M] Mon Nov 30 12:23:36 2015 -0800 "#54" ; git show cb409fc )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo e667cf2... cb409fc:a/README.md [M] Mon Nov 30 12:24:39 2015 -0800 "#55" ; git show cb409fc ) <( echo e667cf2... 1fec082:b/README.md [M] Mon Nov 30 12:24:39 2015 -0800 "#55" ; git show 1fec082 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 5687abc... 1fec082:a/README.md [M] Mon Nov 30 12:25:11 2015 -0800 "#56" ; git show 1fec082 ) <( echo 5687abc... 2d6f7e0:b/README.md [M] Mon Nov 30 12:25:11 2015 -0800 "#56" ; git show 2d6f7e0 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d4bbd6e... 2d6f7e0:a/README.md [M] Mon Nov 30 12:25:50 2015 -0800 "#57" ; git show 2d6f7e0 ) <( echo d4bbd6e... cbc4111:b/README.md [M] Mon Nov 30 12:25:50 2015 -0800 "#57" ; git show cbc4111 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo dd16a90... cbc4111:a/README.md [M] Mon Nov 30 12:29:40 2015 -0800 "#58" ; git show cbc4111 ) <( echo dd16a90... 49b7710:b/README.md [M] Mon Nov 30 12:29:40 2015 -0800 "#58" ; git show 49b7710 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 0d259b9... 49b7710:a/README.md [M] Mon Nov 30 12:33:48 2015 -0800 "#59" ; git show 49b7710 ) <( echo 0d259b9... 3ec130c:b/README.md [M] Mon Nov 30 12:33:48 2015 -0800 "#59" ; git show 3ec130c )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo af4b1b0... 3ec130c:a/README.md [M] Mon Nov 30 12:34:12 2015 -0800 "#60" ; git show 3ec130c ) <( echo af4b1b0... ef99da7:b/README.md [M] Mon Nov 30 12:34:12 2015 -0800 "#60" ; git show ef99da7 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 5d6b830... ef99da7:a/README.md [M] Mon Nov 30 12:34:28 2015 -0800 "#61" ; git show ef99da7 ) <( echo 5d6b830... 8904cc5:b/README.md [M] Mon Nov 30 12:34:28 2015 -0800 "#61" ; git show 8904cc5 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d30059e... 8904cc5:a/README.md [M] Mon Nov 30 12:35:18 2015 -0800 "#62" ; git show 8904cc5 ) <( echo d30059e... 0112734:b/README.md [M] Mon Nov 30 12:35:18 2015 -0800 "#62" ; git show 0112734 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo f7c571a... 0112734:a/README.md [M] Mon Nov 30 12:35:37 2015 -0800 "#63" ; git show 0112734 ) <( echo f7c571a... 3d2c0bc:b/README.md [M] Mon Nov 30 12:35:37 2015 -0800 "#63" ; git show 3d2c0bc )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 8b1b6a1... 3d2c0bc:a/README.md [M] Mon Nov 30 13:00:34 2015 -0800 "#64" ; git show 3d2c0bc ) <( echo 8b1b6a1... 005447d:b/README.md [M] Mon Nov 30 13:00:34 2015 -0800 "#64" ; git show 005447d )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo d2dd279... 005447d:a/README.md [M] Mon Nov 30 13:01:20 2015 -0800 "#65" ; git show 005447d ) <( echo d2dd279... 19d7d14:b/README.md [M] Mon Nov 30 13:01:20 2015 -0800 "#65" ; git show 19d7d14 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 54edeff... 19d7d14:a/README.md [M] Mon Nov 30 13:02:33 2015 -0800 "#66" ; git show 19d7d14 ) <( echo 54edeff... df948af:b/README.md [M] Mon Nov 30 13:02:33 2015 -0800 "#66" ; git show df948af )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo f6c9f56... df948af:a/README.md [M] Mon Nov 30 13:02:44 2015 -0800 "#67" ; git show df948af ) <( echo f6c9f56... 63fe817:b/README.md [M] Mon Nov 30 13:02:44 2015 -0800 "#67" ; git show 63fe817 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo bc1d5d2... 63fe817:a/README.md [M] Mon Nov 30 13:07:15 2015 -0800 "#68" ; git show 63fe817 ) <( echo bc1d5d2... 6b33f13:b/README.md [M] Mon Nov 30 13:07:15 2015 -0800 "#68" ; git show 6b33f13 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 05222e9... 6b33f13:a/README.md [M] Mon Nov 30 15:35:07 2015 -0800 "#69" ; git show 6b33f13 ) <( echo 05222e9... 30111f2:b/README.md [M] Mon Nov 30 15:35:07 2015 -0800 "#69" ; git show 30111f2 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo 9514990... 30111f2:a/README.md [M] Mon Nov 30 15:35:46 2015 -0800 "#70" ; git show 30111f2 ) <( echo 9514990... e9e6cc9:b/README.md [M] Mon Nov 30 15:35:46 2015 -0800 "#70" ; git show e9e6cc9 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo da76c96... e9e6cc9:a/README.md [M] Mon Nov 30 16:18:56 2015 -0800 "#71" ; git show e9e6cc9 ) <( echo da76c96... 7ace2f1:b/README.md [M] Mon Nov 30 16:18:56 2015 -0800 "#71" ; git show 7ace2f1 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo f2fef8d... 7ace2f1:a/README.md [M] Tue Dec 1 12:36:37 2015 -0800 "#72" ; git show 7ace2f1 ) <( echo f2fef8d... adb5525:b/README.md [M] Tue Dec 1 12:36:37 2015 -0800 "#72" ; git show adb5525 )
vimdiff -u /Users/blarf/.my-vimdiffrc <( echo da6d560... adb5525:a/README.md [M] Tue Dec 1 12:37:49 2015 -0800 "#73" ; git show adb5525 ) <( echo da6d560... 632cdcf:b/README.md [M] Tue Dec 1 12:37:49 2015 -0800 "#73" ; git show 632cdcf )
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
