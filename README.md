# Notes

## Shell Scripting

### Creating RSA Key Pair

```bash
ssh-keygen -t rsa
```

### Git

Git undo last commit and keep working changes

```bash
git reset --soft HEAD~
```

Git squash last `2` commits
Will undo the last `2` commits and keep the changes staged and ready for another commit

```bash
git reset --soft HEAD~2
git commit -m "Some feature"
```

### Sync Files

Use [rSync](http://linux.die.net/man/1/rsync) to sync files over ssh

```bash
# note the trailing slash at the end of `~/Desktop/` to indicate all contents
rsync -avzP ~/Desktop/ awesome@192.168.1.1:/home/awesome/files
```
