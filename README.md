# Notes

## Shell Scripting

### Creating RSA Key Pair

```bash
ssh-keygen -t rsa
```

### Git

Git squash last `2` commits

```bash
git reset --soft HEAD~2
```

### Sync Files

Use [rSync](http://linux.die.net/man/1/rsync) to sync files over ssh

```bash
# note the trailing slash at the end of `~/Desktop/` to indicate all contents
rsync -avzP ~/Desktop/ awesome@192.168.1.1:/home/awesome/files
```
