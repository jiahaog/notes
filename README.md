# Notes

## Shell

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

### Run a shell command forever

```bash
while true; do npm start; done
```

### Bash

#### Quotations

```
SOME_VAR="Hello"

echo "${SOME_VAR}" # 'Hello'
echo '${SOME_VAR}' # '${SOME_VAR}'
echo `${SOME_VAR}` # '-bash: Hello: command not found'
```

#### Best Practice
- Immediately exits script on error
- Performs clean up on error

```bash
#!/bin/sh

# exit the shell script on error immediately
set -e

TEMP_DIR="SOME_TEMP_DIR"

function cleanUp() {
  rm -rf "${TEMP_DIR}"
}

# Set up cleanUp to trigger on EXIT
trap cleanUp EXIT

mkdir -p "${TEMP_DIR}"

# Do things
# Error here will trigger `cleanUp()` and exit the script immediately
echo 'I am awesome'

# Done, remove the cleanup trap
trap - EXIT

# Manual Cleanup
cleanUp
```

#### Executing Scripts

`source` runs the script in the current process, does not require the script to be an executable

```bash
$ source some_script
$ . some_script # same as the above
```

`./some_script` spawns a new shell and executes the script, according to the shebang line

`bash some_script` also spawns a new shell and executes the file with bash

## Javascript

### Creating Objects

```javascript
const personPrototype = {
  hello() {
    return `Hello, my name is ${this.name}`;
  }
}

const george = Object.create(personPrototype);

george.name = 'George';
george.hello()
```
[Source](https://youtu.be/lKCCZTUx0sI?t=15m1s)

See also:
- [Different ways to create objects](https://np.reddit.com/r/javascript/comments/4c7dfn/which_way_is_the_best_way_to_create_objects_in/d1fp9kl)
