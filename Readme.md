# shipit :shipit:

Minimalistic SSH deployment.

![shipit](https://d3vv6lp55qjaqc.cloudfront.net/items/0c1z211n0b1m3d1g370u/shipit.png)

## Installation

Copy this one-liner and run it in your shell:

```bash
SHPT=/usr/local/bin/shipit && curl -o $SHPT https://raw.githubusercontent.com/sapegin/shipit/master/bin/shipit && chmod +x $SHPT && unset SHPT
```

You can use this command to update shipit too.

**Note:** Use `sudo` or replace `/usr/local/bin/shipit` to path somewhere inside your home directory.

## Usage

```bash
shipit [option] <command>
```

### Commands

| Command         | Aliases | Description |
| --------------- | ------- | ----------- |
| `<target>`      | | Execute `target` target on the remote host |
| list            | ls | Print list of available targets |
| console         | shell, ssh | Open an SSH session on remote host |
| exec `<cmd>`    | run | Execute `cmd` on the remote host |
| copy `<file>`   | cp | Copy `files` to the remote host |
| --version       | -V | Print shipit version |
| --help          | -h | Print help |

### Options

| Option          | Description |
| --------------- | ----------- |
| -c, --config    | Config file path (default: `.shipit`) |
| -v, --verbose   | Enable verbose mode for SSH |

### Examples

Execute `deploy` target:

```bash
shipit
```

Execute `status` target:

```bash
shipit status
```

Show a list of available targets:

```bash
shipit list
```

Execute `uptime` command on the remote host:

```bash
shipit exec uptime
```

## Configuration

You need to create `.shipit` file in your project’s directory.

Here is a typical config:

```ini
host='myhost'
path='sites/example.com'

[deploy:local]
git push origin master

[deploy]
git checkout master
git pull
npm install
grunt build

[status]
uptime
```

The only required things are `host` and `path` parameters, and `[deploy]` or `[deploy:local]` target.

For non-standard port number and to specify which SSH key to use, edit your SSH config, `~/.ssh/config`:

```
host example.com
IdentityFile ~/.ssh/keyfile
port 10022
user usernamehere
```

### Parameters

### host

It’s the same host you use in `ssh` command. It could be string of format `<username>@<ip>:<port>` or just a name of `~/.ssh/config` record.

### path

Project path on remote host. shipit will `cd` to this directory before executing any command.

### Targets

Target is just a bunch of shell command that will be executed on remote host via SSH. You can define as many targets as you want.

Note that you can’t use blank lines inside targets but you can use comments (#) and other things—it’s just a shell script.

### Local targets

If you append `:local` to a target name (like `[name:local]`) it will be executed on your local machine before remote target with the same name. You can define only local, only remote or both targets.

In case of any errors in local target remote target won’t be executed.

You can use these variables:

* `$SSH_HOST` — your config’s `host` value,
* `$SSH_PATH` — your config’s `path` value.

## Examples

### Deploy from Git

```ini
host='myhost'
path='sites/example.com'

[deploy:local]
git push origin master

[deploy]
git checkout master
git pull
npm install
npm prune
npm run build
```

### Deploy with `rsync`

```ini
host='myhost'
path='sites/example.com'

[deploy:local]
npm test
npm run build
rsync --archive --compress --force --delete public/ $SSH_HOST:$SSH_PATH
```

## Changelog

The changelog can be found on the [Releases page](https://github.com/sapegin/shipit/releases).


---

## License

The MIT License, see the included [License.md](License.md) file.
