# gpgedit

Edit a gpg encrypted file.  Create a tmp directory under $HOME.  Decrypt the encrypted file to the tmp directory.  Edit the decrypted file.  (The default editor is vim, but the variables EDITOR and VISUAL in the environment are checked--in that order.)  If changes are detected in the edited file, re-encrypt over the original encrypted file.

# usage

```usage: gpgedit File```
