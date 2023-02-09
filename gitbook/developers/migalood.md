# Migalood Installation and setup

## Choose an Operating System

The operating system you use for your node is entirely your personal preference. You will be able to compile the 
`migaloo` daemon on most modern linux distributions and recent versions of macOS.

{% hint style="info" %}
For the tutorial, it is assumed that you are using an Ubuntu LTS release.

If you have chosen a different operating system, you will need to modify your commands to suit your operating system.
{% endhint %}

## Install pre-requisites

```bash
# update the local package list and install any available upgrades
sudo apt-get update && sudo apt upgrade -y

# install toolchain and ensure accurate time synchronization
sudo apt-get install make build-essential gcc git jq chrony -y
```

## Install Go

Follow the instructions [here](https://golang.org/doc/install) to install Go. Please install Go v1.20 or later.

For an Ubuntu LTS, you can probably use:

```bash
wget https://golang.org/dl/go1.20.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
```

Unless you want to configure in a non-standard way, then set these in the `.profile` in the user's home (i.e. `~/`) folder.

```bash
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

After updating your `~/.profile` you will need to source it:

```bash
source ~/.profile
```

## Build Migaloo from source

```bash
git clone git@github.com:White-Whale-Defi-Platform/migaloo-chain.git
cd migaloo-chain
git fetch
git checkout <version-tag>
```

Migaloo's mainnet is not live yet. Once it is launched, you can review the latest mainnet version tag [here](https://github.com/White-Whale-Defi-Platform/migaloo-chain/releases).

{% hint style="warning" %}
The testnet tag is `v1.0.0-rc0` - i.e:

```bash
git checkout v1.0.0-rc0
```
{% endhint %}

Once you're on the correct tag, you can build:

```bash
# in migaloo-chain dir
make install
```

To confirm that the installation has succeeded, you can run:

```bash
which migalood
# Should return similar to:
# /home/<username>/go/bin/migalood

migalood version
# Will return the version number of the branch checked out above
```

## Running a Node

Continue with [Join the Mainnet](../validators/mainnet.md) or [Join the Testnet](../validators/testnet.md).