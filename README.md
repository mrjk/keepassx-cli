# KeepassX-CLI

A dumb KeepassXC reader for shell scripts, with automated unlock. This script exists because Keepass database tends
to be difficult to use programmatically in scripts or in automation.

## Features

`keepassx-cli` provides the following features:

- Manage keepass database password in local keyring
- Allow to use different databases
- Very flexible CLI
- Easy secret discovery
- Profile management

Last stable version is: `0.0.2`

## 💻 Installation

### Dependencies

You need to have [keepassxc](https://keepassxc.org/) installed.

### Install via ASDF or Mise

Can be installed by both `mise` or `asdf`:
```bash
mise plugin install keepassx-cli https://github.com/mrjk/asdf-keepassx-cli.git
mise use keepassx-cli

# With asdf
asdf plugin add keepassx-cli https://github.com/mrjk/asdf-keepassx-cli.git
asdf install keepassx-cli latest
asdf global keepassx-cli latest
```

### Install via Curl

Install it via curl:
```bash
curl -o keepassx-cli https://raw.githubusercontent.com/mrjk/keepassx-cli/main/keepassx-cli
chmod +x keepassx-cli
mv keepassx-cli /usr/local/bin/keepassx-cli
```

### Install from Source

Install via git:
```bash
git clone https://github.com/mrjk/keepassx-cli.git
cd keepassx-cli
chmod +x keepassx-cli
ln -s keepassx-cli /usr/local/bin/keepassx-cli
```

### Verify installation

Should be able to get `keepassx-cli` version and help message:
```bash
keepassx-cli --version
keepassx-cli --help
```

## 🚀 Quickstart

### Basic Usage

Basic usage looks like:
```
keepassx-cli COMMAND PROFILE ARGS...
```

Fetch a password of a given Keepass key name:
```
$ keepassx-cli get perso Personal / github.com / user
Pr1v4TePassword
$ keepassx-cli get pro Corp.com / devops / gitlab.com / buildsvc
Person4alC0rpP4ssw0rd
```

Everything after `get` is considered as a single argument, and spaces does not really matters. Following
statements are equivalent:
```
keepassx-cli get pro Corp.com / devops / gitlab.com / buildsvc
keepassx-cli get pro "Corp.com / devops / gitlab.com / buildsvc"
keepassx-cli get pro "Corp.com/devops/gitlab.com/buildsvc"
keepassx-cli get pro Corp.com/devops/gitlab.com/buildsvc
```

It is possible to see the full entry with:
```
keepassx-cli show pro Corp.com / devops / gitlab.com / buildsvc
```

Fetch attachment:
```
$ keepassx-cli extract pro "Corp.com / devops / infra / certificates" wildcard-cert.key
-----BEGIN CERTIFICATE-----
...
```

Show database as tree or list:
```
keepassx-cli tree pro
keepassx-cli dump pro
```

    Notes: The `dump` and `tree` commands remove duplicates entries

Finally, to enable by default a single profile in your shell (ie, with direnv):
```
eval "$(keepassx-cli shell john)"
```

## Usage

There are some key concepts to work with `keepassx-cli`.

### Configuration

A default configuration is `/.config/keepassx-cli/conf.env`. This configuration is optional, and it is used when no profile is used (see below).


It is always possible to edit configuration with your favorite text editor, the configuration syntax currently accept 2 keys. 

```
$ cat ~/.config/keepassx-cli/conf.env
# Path of the kbdx file
KC_DB=$HOME/Documents/MyVault.kbdx

# Password to unlock kbdx file
# Note: Always prefer to use keyring instead of clear text passwords.
# Leave empty to use keyring or prompt
KC_PASS=
```

    Note: There is no way to save kbdx password in keyring without profile.

### Profile management

Profiles should be managed by the CLI. Profiles live in `~/.config/keepassx-cli/`. Each profile owns its own configuration, and the configuration file `~/.config/keepassx-cli/conf.john.env` must match the `john` profile name.

To create a new profile:
```
keepassx-cli profile add john
```

To list profiles:
```
keepassx-cli profile ls
```

To save profile password (aka kbdx password):
```
keepassx-cli profile password john
```
Password will ideally be saved in local keyring, or directly in clear text in the profile config file.


### Command line usage

Command line usage:
```
$ keepassx-cli --help
keepassx-cli is command line tool

usage: keepassx-cli [OPTS] COMMAND [ARGS]
       keepassx-cli help

commands:
  dump                    [PROFILE] [PATTERN]   Dump all keys
  extract                 [PROFILE] KEY FILE    Extract attachement from key
  get                     [PROFILE] KEY         Get key secret
  info                                          Show debug info
  profile                 COMMAND               Manage profiles
  profile add             PROFILE [PATH_KBDX]   Add new profile
  profile edit            PROFILE               Edit profile configuration
  profile ls                                    List profiles
  profile password        PROFILE               Save database password in keyring
  profile rm              PROFILE               Remove profile
  profile show            PROFILE               Show profile configuration
  shell                   [PROFILE]             Generate shell code to be sourced in shell
  show                    [PROFILE] KEY         Show a key
  tree                    [PROFILE] [GROUP]     Dump as tree

options:
  -P|--profile PROFILE    Set query profile
  -k|--key ID             Set key to retrieve
  -d|--db FILE            Path to kbdx file
  -p|--pass PASSWORD      Keepass db password
  -K|--keyring BOOL       Enable keyring agent to fetch password
  -t|--prompt             Prompt password if missing
  -f|--force              Enable force mode
  -h|--help               Show this help message
  -v|-vv|-vvv|--verbose [LEVEL]  Set verbosity level
  -V|--version            Show version

info:
  config dir: ~/.config/keepassx-cli
  author: mrjk <mrjk<dot>78<at>gmail<dot>com>
  version: 0.0.2-stable (2024-07-25)
  license: GPLv3

```

### Troubleshooting

You can get information about your configuration with:
```
$ keepassx-cli info john
KP_BIN=flatpak run --command=/app/bin/keepassxc-cli org.keepassxc.KeePassXC

KEEPASSX_CLI__KEY=
KEEPASSX_CLI__PROFILE=john
KEEPASSX_CLI__DB=/home/john/Documents/robin/john_pass.kdbx
KEEPASSX_CLI__CONF=/home/john/.config/keepassx-cli/conf.john.env
KEEPASSX_CLI__PASS=

APP_REAL_NAME=keepassx-cli
SCRIPT_PATH=/home/john/.local/scripts/keepassx-cli
SCRIPT_DIR=/home/john/.local/scripts
SCRIPT_REAL_PATH=/home/john/prj/john/work/apps/keepassx-cli/keepassx-cli
SCRIPT_REAL_DIR=/home/john/prj/john/work/apps/keepassx-cli
KEEPASS_BIN=flatpak run --command=/app/bin/keepassxc-cli org.keepassxc.KeePassXC
```

## Informations

### Known issues

* No support for OTP yet
* Rework default config and allow usage of local keyring

### Alternatives

Other alternatives than this project:

- https://github.com/hargoniX/keepassxc-proxy-client
- https://github.com/wzykubek/rofi-keepassxc
- https://github.com/Frederick888/git-credential-keepassxc

### License

GPLv2


