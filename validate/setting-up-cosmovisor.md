# Setting up Cosmovisor

### What is Cosmovisor?

Cosmovisor is a small process manager for Cosmos SDK application binaries that monitors the governance module for incoming chain upgrade proposals. If it sees a proposal that gets approved, Cosmovisor can automatically download the new binary, stop the current binary, switch from the old binary to the new one, and finally restart the node with the new binary.

## Setting up Cosmovisor <a href="#setting-up-cosmovisor" id="setting-up-cosmovisor"></a>

Setting up Cosmovisor is relatively straightforward. However, it does expect certain environment variables and folder structure to be set.

Cosmovisor allows you to download binaries ahead of time for chain upgrades, meaning that you can do zero (or close to zero) downtime chain upgrades. It's also useful if your local timezone means that a chain upgrade will fall at a bad time. Rather than having to do stressful ops tasks late at night, it's always better if you can automate them away, and that's what Cosmovisor tries to do.

### First, go and get Cosmovisor (**recommended approach**):

```
go get cosmossdk.io/tools/cosmovisor

# or, with go >= 1.15 you can do.  This is the most common:
go install cosmossdk.io/tools/cosmovisor@latest

# If required you can target a specific version:
go install cosmossdk.io/tools/cosmovisor@v1.4.0
```

**Your installation can be confirmed with:**

```
which cosmovisor
```

**This will return something like:**

```
/home/<your-user>/go/bin/cosmovisor
```

Building from source allows you to target a specific version of Cosmovisor, in case you do not want to run 1.4.0.

You can also build from source; cosmovisor is in the main `cosmos-sdk` repo on Github, so you can use Git tags to target a specific version. This example uses a tag, `v0.42.7` that refers to the Cosmos SDK, as Cosmovisor-specific tags did not exist before August 2021. The first of these was `cosmovisor/v0.1.0`, and the second is the current release, `cosmovisor/v1.0.0`.

```
git clone https://github.com/cosmos/cosmos-sdk
cd cosmos-sdk
git checkout v0.42.7
make cosmovisor
cp cosmovisor/cosmovisor $GOPATH/bin/cosmovisor
cd $HOME
```

### Add environment variables to your shell <a href="#add-environment-variables-to-your-shell" id="add-environment-variables-to-your-shell"></a>

In the `.profile` file, usually located at `~/.profile`, add:

```
export DAEMON_NAME=starsd
export DAEMON_HOME=$HOME/.starsd
```

**Then source your profile to have access to these variables:**

```
source ~/.profile
```

**You can confirm success like so:**

```
echo $DAEMON_NAME
```

It should return `migalood`.

## Set up folder structure <a href="#set-up-folder-structure" id="set-up-folder-structure"></a>

Cosmovisor expects a certain folder structure as shown below. You can install the Linux utility `tree` to produce a similar output to check on your own server.

```
.
├── current -> genesis or upgrades/<name>
├── genesis
│   └── bin
│       └── $DAEMON_NAME
└── upgrades
    └── <name>
        └── bin
            └── $DAEMON_NAME
```

Don't worry about `current` - that is simply a symlink used by Cosmovisor. The other folders will need setting up, but this is easy:

```
mkdir -p $DAEMON_HOME/cosmovisor/genesis/bin
mkdir -p $DAEMON_HOME/cosmovisor/upgrades
```

### Set up Genesis binary <a href="#set-up-genesis-binary" id="set-up-genesis-binary"></a>

Cosmovisor needs to know which binary to use at Genesis. We put this in `$DAEMON_HOME/cosmovisor/genesis/bin`.

**First, find the location of the binary you want to use:**

```
which migalood
```

**Then use the path returned to copy it to the directory Cosmovisor expects. Let's assume the previous command returned `/home/your-user/go/bin/migalood`:**

```
cp /home/<your-user>/go/bin/migalood $DAEMON_HOME/cosmovisor/genesis/bin
```

Once you're done, check the folder structure looks correct using a tool like `tree`.

### Set up systemd service <a href="#set-up-systemd-service" id="set-up-systemd-service"></a>

Commands sent to Cosmovisor are sent to the underlying binary. For example, `cosmovisor version` is the same as typing `migalood version`.

Nevertheless, just as we would manage `migalood` using a process manager, we would like to make sure Cosmovisor is automatically restarted if something happens, for example, an error or reboot.

**First, create the service file:**

```
sudo nano /etc/systemd/system/cosmovisor.service
```

Change the contents of the below to match your setup - `cosmovisor` is likely at `~/go/bin/cosmovisor` regardless of which installation path you took above, but it's worth checking.

```
[Unit]
Description=cosmovisor
After=network-online.target

[Service]
User=<your-user>
ExecStart=/home/<your-user>/go/bin/cosmovisor start
Restart=always
RestartSec=3
LimitNOFILE=65535
Environment="DAEMON_NAME=migalood"
Environment="DAEMON_HOME=/home/<your-user>/.migalood"
Environment="DAEMON_ALLOW_DOWNLOAD_BINARIES=false"
Environment="DAEMON_RESTART_AFTER_UPGRADE=true"
Environment="DAEMON_LOG_BUFFER_SIZE=512"

[Install]
WantedBy=multi-user.target
```

A description of what the environment variables do can be found [here](https://docs.cosmos.network/master/run-node/cosmovisor.html). Change them depending on your setup.

Note also that we set buffer size explicitly because of a [live bug in Cosmovisor](https://github.com/cosmos/cosmos-sdk/pull/8590) before version `v1.0.0`. If you are using `v1.0.0`, you may omit that line.

### Start Cosmovisor <a href="#start-cosmovisor" id="start-cosmovisor"></a>

Finally, enable the service and start it.

```
sudo -S systemctl daemon-reload
sudo -S systemctl enable cosmovisor
sudo systemctl start cosmovisor
```

Check it is running using:

```
sudo systemctl status cosmovisor
```

If you need to monitor the service after launch, you can view the logs using:

```
journalctl -u cosmovisor -f
```
