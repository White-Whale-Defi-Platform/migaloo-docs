# Installing migalood

## Choose an Operating System <a href="#choose-an-operating-system" id="choose-an-operating-system"></a>

The operating system you use for your node is entirely your personal preference. You will be able to compile the `Migaloo` daemon on most modern Linux distributions and recent versions of macOS.

{% hint style="info" %}
For the tutorial, it is assumed that you are using an Ubuntu LTS release.

If you have chosen a different operating system, you will need to modify your commands to suit your operating system.
{% endhint %}

## Install prerequisites

{% tabs %}
{% tab title="Update the Local Package" %}
```markdown
sudo apt-get update && sudo apt upgrade -y
```
{% endtab %}

{% tab title="Install Toolchain" %}
```markdown
sudo apt-get install make build-essential gcc git jq chrony -y
```
{% endtab %}
{% endtabs %}

## Install Go Lang

Follow the instructions [here](https://golang.org/doc/install) to install Go. Please install Go v1.20 or later.

For an Ubuntu LTS, you can probably use:

```markdown
wget https://golang.org/dl/go1.20.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && tar -C /usr/local -xzf go1.20.linux-amd64.tar.gz
```

Unless you want to configure in a non-standard way, then set these in the `.profile` in the user's home (i.e. `~/`) folder.

```markdown
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
```

After updating your `~/.profile` you will need to source it:

```markdown
source ~/.profile
```

### Build Migaloo from source <a href="#build-migaloo-from-source" id="build-migaloo-from-source"></a>

```markdown
git clone git@github.com:White-Whale-Defi-Platform/migaloo-chain.git
cd migaloo-chain
git fetch
git checkout <version-tag>
```

Migaloo's current Mainnet tag can be found here: [https://github.com/White-Whale-Defi-Platform/migaloo-chain/releases](https://github.com/White-Whale-Defi-Platform/migaloo-chain/releases)

### Once you're on the correct tag, you can build:

```markdown
# in migaloo-chain directory
make install
```

### To confirm that the installation has succeeded, you can run:

```markdown
which migalood
# Should return similar to:
# /home/<username>/go/bin/migalood

migalood version
# Will return the version number of the branch checked out above
```

Should you come across any issue when installing migalood, please reach out via the Developer Channel on [Discord](https://discord.gg/bJE7hxJ6sE)
