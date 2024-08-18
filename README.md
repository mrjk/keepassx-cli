# KeepassX-CLI

A shell script to read easily secrets and attachment in KeepassXC.

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

TODO
