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

Rebase local branch with remote master

```bash
# while on the local branch

git fetch origin
git rebase origin/master
```

Replace messed up local branch with remote master

```bash
git reset --hard origin/master
```

Delete branches

```
# delete local branch
git branch -d <branchName>

# delete remote branch
git push origin --delete <branchName>
```

#### Submodules

```
# git clone with submodules
git clone --recursive <submodule repository>

# update submodule from upstream
git submodule update --remote <submodule>

# To pull in changes from submodules after the root repository remote has been updated
git pull
git submodule update
```

### Sync Files

Use [rSync](http://linux.die.net/man/1/rsync) to sync files over ssh

```bash
# note the trailing slash at the end of `~/Desktop/` to indicate all contents
rsync -avzP ~/Desktop/ awesome@192.168.1.1:/home/awesome/files
```

Also can use scp

```bash
scp -r user@192.168.1.1:/home/data/  ~/Desktop/
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

#### Multiple Commands in One Line

```bash
A; B    # Run A and then B, regardless of success of A
A && B  # Run B if A succeeded
A || B  # Run B if A failed
A &     # Run A in background.
```

[Source](http://askubuntu.com/a/539293)

#### Redirecting Output

```bash
do_stuff > /dev/null 2>&1
```
Redirects `stdout` to `/dev/null`, redirects `stderr` to `stdout`

[Source](http://unix.stackexchange.com/a/119650)

#### Test TCP Connection

Helpful snippet to test network connections that do not respond to cUrl

```bash
(echo > /dev/tcp/$HOST/$PORT) # returns a status code of whether the connection is successful
echo $? # prints the status code of the last command
```
#### Find Process Running on Port

```bash
lsof -i tcp:8002
```

#### OSX Clipboard

Copy stdout to clipboard

```bash
$ echo 'Hello!' | pbcopy
```

### Caffeinate

Prevent OSX from sleeping

```bash
$ caffeinate

# Or prevent sleep until script has completed
$ caffeinate long_running_script.sh
```

#### Simple HTTP Server

Creates a HTTP server for files in the local directory. Either an `index.html` file (if present) or the other files in the directory wil be served.

```bash
$ python -m SimpleHTTPServer 8080
```

#### Find Files

Find files by filename or wildcard

```bash
$ find ./test -name "*.php"
```

#### Download a Static Website

```bash
$ wget -r -N http://example.com
```

#### Keyboard Shortcuts

| Shortcut      | Purpose                                   |
|---------------|-------------------------------------------|
| `ctrl` + `a`  | Go to the beginning of the line           |
| `ctrl` + `e`  | Go to the end of the line                 |
| `ctrl` + `u`  | Clear everything before the cursor        |
| `ctrl` + `w`  | Delete the word before the cursor         |
| `ctrl` + `l`  | Clear the screen                          |


#### Miscellaneous Commands
```bash
# show hex and ascii values
$ xxd some_file.mp3

# DNS address lookup
$ nslookup google.com

# show local cached IP-MAC address table
$ arp -a

# copy contents of one directory to another
$ cp directory_with_desired_contents/* ~/destination_directory
```

## OSX

### Taking screenshots

| Shortcut      | Purpose                                   |
|---------------|-------------------------------------------|
| ⌘ + ⌃ + ⇧ + 4 | Select **area** and save to **clipboard** |
| ⌘ + ⇧ + 4     | Select **area** and save to **desktop**   |
| ⌘ + ⌃ + ⇧ + 3 | Save **screen** to **clipboard**          |
| ⌘ + ⇧ + 3     | Save **screen** to **desktop**            |

While selecting area, press `space` to select a window.


## Javascript

### Factory Pattern for Objects

```javascript
function createObject(objectParams) {
  const prototype = {
    instanceMethod(params) {
      return params;      
    }
  };

  const instance = Object.create(prototype);

  const instanceProps = {
    data: objectParams.data
  };
  return Object.assign(instance, instanceProps);
}
```

See also:
- [Different ways to create objects](https://np.reddit.com/r/javascript/comments/4c7dfn/which_way_is_the_best_way_to_create_objects_in/d1fp9kl)

## Python
### Run a prompt after running a python file
```
python -i hello.py
```

### Function Arbitrary arguments `*args`

```python
def foo(*args):

    def bar(*args):
        return args
        
    return bar(*args)

>>> foo(1,2,3) # (1, 2, 3)
```

## MongoDB

MongoDB Authentication

```
# first login with admin credentials
use admin
db.auth('admin', 'password')

use myDatabase

# create a user local to `myDatabase`
db.createUser({ user: "myuser",
  pwd: "password",
  roles: [
    "readWrite"
  ]
})

```

Connect to the database using the following:
```
MONGO_URL=mongodb://${DATABASE_USER}:${DATABASE_PASSWORD}@${MONGO_IP}/${DATABASE_NAME}
```

## Docker

### Docker Compose 

Rebuild and recreate all containers
```
docker-compose up -d --force-recreate --build
```

### Connecting to host (not VM) inside the docker container

On host (Mac):
```bash
ipconfig getifaddr en0
```

## Performance

Benchmark a command

```sh
perf stat -r 10 $command
```
