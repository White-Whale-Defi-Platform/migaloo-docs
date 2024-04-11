# Join the Testnet

Here is a list of Migaloo Testnets along with their current status. You will need to know the version tag for installation of the `migalood` binary. To install `migalood`, follow the instructions in the [installing migalood](../develop/cli/installing-migalood.md) section.

To keep up with upgrades and syncing information on the active Testnet, you can refer to the Testnet's repository, which is the definitive source of truth.

If you encounter any difficulties, don't hesitate to seek assistance on [Discord](https://discord.com/invite/tSxyyCWgYX).

| chain-id  | Version Tag        | Status | RPC                                       | API                                       |
| --------- | ------------------ | ------ | ----------------------------------------- | ----------------------------------------- |
| narwhal-2 | v4.1.0-testnet-rc2 | Active | https://migaloo-testnet-rpc.polkachu.com/ | https://migaloo-testnet-api.polkachu.com/ |

For information like State-Sync, Node Snapshots and more, please refer to [Polkachu's Migaloo Testnet page](https://polkachu.com/testnets/migaloo).

### Minimum Hardware Requirements <a href="#minimum-hardware-requirements" id="minimum-hardware-requirements"></a>

Operating System: Linux or macOS Disk Space: At least 100GB of free space is recommended. CPU: Multi-core processor, 4+ cores recommended RAM: 8GB+ recommended Network: Good internet connectivity

{% hint style="info" %}
These specifications are the minimum recommended. As Migaloo Network is an open smart contract platform, it can at times be very demanding on hardware. Low spec validators WILL get stuck on difficult to process blocks.
{% endhint %}

{% hint style="info" %}
It is important to note that the data on testnets increases as the blockchain progresses, thus requiring an expansion of storage as the blockchain database grows over time.
{% endhint %}

### Configuration of Shell Variables <a href="#configuration-of-shell-variables" id="configuration-of-shell-variables"></a>

For this guide, we will be using shell variables. This will enable the use of the client commands verbatim. It is important to remember that shell commands are only valid for the current shell session, and if the shell session is closed, the shell variables will need to be re-defined.

If you want variables to persist for multiple sessions, then set them explicitly in your shell `.profile`, as you did for the Go environment variables.

To clear a variable binding, use `unset $VARIABLE_NAME`. Shell variables should be named with ALL CAPS.

### Choose a testnet <a href="#choose-a-testnet" id="choose-a-testnet"></a>

Set the `CHAIN_ID`:

```
CHAIN_ID=narwhal-2
```

### Set your moniker name <a href="#set-your-moniker-name" id="set-your-moniker-name"></a>

Choose your `<moniker-name>`, this can be any name of your choosing and will identify your validator in the explorer. Set the `MONIKER_NAME`:

```
MONIKER_NAME=<moniker-name>

#Example
MONIKER_NAME="Moby Dick Validator"
```

### Setting up the Node <a href="#setting-up-the-node" id="setting-up-the-node"></a>

{% hint style="info" %}
Running a node is different from running a Validator. In order to run a Validator, you must create and sync a node, and then upgrade it to a Validator.
{% endhint %}

These instructions will direct you on how to initialise your node, synchronise to the network and upgrade your node to a validator.

### **Initialize the chain** <a href="#initialize-the-chain" id="initialize-the-chain"></a>

```
migalood init $MONIKER_NAME --chain-id $CHAIN_ID
```

This will generate the following files in `~/.migalood/config/`

* `genesis.json`
* `node_key.json`
* `priv_validator_key.json`

{% hint style="info" %}
Note that this means if you jumped ahead and already downloaded the genesis file, this command will replace it and you will get an error when you attempt to start the chain.
{% endhint %}

### Download the Genesis file <a href="#download-the-genesis-file" id="download-the-genesis-file"></a>

```
curl https://raw.githubusercontent.com/White-Whale-Defi-Platform/migaloo-networks/main/narwhal-2/genesis.json > ~/.migalood/config/genesis.json
```

This will replace the genesis file created using `migalood init` command with the genesis file for the Testnet.

### **Set persistent peers** <a href="#set-persistent-peers" id="set-persistent-peers"></a>

Persistent peers will be required to tell your node where to connect to other nodes and join the network. To retrieve the peers for the chosen Testnet, visit [Polkachu's Peer page](https://polkachu.com/testnets/whitewhale/peers).

```
#Set the base repo URL for the testnet & retrieve peers
PEERS=bba98e78698762439b7b44890d020ca4e907b2ba@65.109.113.130:26656,02eb3672077b55c768722db59c117148b858fcd6@107.155.81.114:26656,345d080ab5f4913dae5ff25398d430a52ec75718@116.202.216.113:2000,c3f889bc93d214bbf74e0f41fad263680141a0be@136.243.88.91:3340,7b0ed0c2c62e3bedc000c133a009db477a3b4345@144.76.67.53:2550,236d988e8309dd21472c53ff575865d7558aad31@51.210.223.185:37095,4b491559cf47bc3742d271fec59edc079483ee3b@88.99.3.158:20756,ca412eeff90f68757c26100263a9eb7b43027ae3@65.109.52.178:26656,ff9608cf25564d4695c5cea3f248f81bd570ae19@159.69.194.159:26656,8e04e9183e497560248155fb4266cd02d71fcb27@38.146.3.202:20756,d3e972f5ce127e050c7f940a1ce272c76de483b6@65.144.145.234:26656,b583943b94d3e9a12fe6425684eeee1f8bf42934@142.132.209.236:20756,73700a6e427b1ac51ccec3906091d7e2d5d175b0@95.217.144.107:20756,62a9c8d2a94cd127bc19f26eaa686741b221eb67@148.251.245.158:26656,7c04ce8a7aab9ff4d4d6049fc8a4870d6ecb7c25@65.21.232.185:2000,ed5bb09be55afecc9a32844bb53102fda3b94cee@142.132.154.176:3000,317d44d53b0b67aa040962813637fee139540f34@51.81.57.80:20756,df7806813f798816c0c19151160ad544e7013039@54.174.174.229:26656,bec6c2f30b1f7621f5f83bffb317d74939240c5c@141.95.110.235:26656,ade4d8bc8cbe014af6ebdf3cb7b1e9ad36f412c0@176.9.82.221:20756

# check it worked
echo $PEERS

#Update persistent_peers setting in config.toml.
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.migalood/config/config.toml
```

### Set minimum gas prices <a href="#set-minimum-gas-prices" id="set-minimum-gas-prices"></a>

In `$HOME/.migalood/config/app.toml`, set gas prices:

```
#note testnet denom
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0uwhale\"/" ~/.migalood/config/app.toml
```

### **Create a local key pair** <a href="#create-a-local-key-pair" id="create-a-local-key-pair"></a>

Create a new key pair or restore a key for your validator:

```
# Create new keypair
migalood keys add <key-name>

# Restore existing migaloo wallet with mnemonic seed phrase.
# You will be prompted to enter mnemonic seed.
migalood keys add <key-name> --recover

# Query the keystore for your public address
migalood keys show <key-name> -a
```

Replace `<key-name>` with a key name of your choosing.

{% hint style="danger" %}
After creating a new key, the key information and seed phrase will be shown. It is essential to write this seed phrase down and keep it in a safe place. The seed phrase is the only way to restore your keys.
{% endhint %}

### **Get some testnet tokens** <a href="#get-some-testnet-tokens" id="get-some-testnet-tokens"></a>

Testnet tokens can be requested from the `#testnet-faucet` channel on [Discord](https://discord.com/channels/908044702794801233/1069611287149039718).

To request tokens type `$request <your-public-address> narwhal` in the message field and press enter.

### Setup cosmovisor <a href="#setup-cosmovisor" id="setup-cosmovisor"></a>

Follow [these](https://github.com/White-Whale-Defi-Platform/migaloo-docs/blob/main/gitbook/validators/gitbook/validators/cosmovisor.md) instructions to setup cosmovisor and start the node.

### Syncing the node <a href="#syncing-the-node" id="syncing-the-node"></a>

After starting the `migalood` daemon, the chain will begin to sync to the network. The time to sync to the network will vary depending on your setup, but could take a very long time. To query the status of your node:

```
# Query via the RPC (default port: 26657)
curl http://localhost:26657/status | jq .result.sync_info.catching_up
```

If this command returns `true` then your node is still catching up. If it returns `false` then your node has caught up to the network current block and you are safe to proceed to upgrade to a validator node.

{% hint style="info" %}
Validators and sentries can rapidly join the network with state-sync. See instructions for using state-sync [here](https://github.com/White-Whale-Defi-Platform/migaloo-docs/blob/main/gitbook/validators/gitbook/validators/state\_sync.md).
{% endhint %}

### Upgrade to a validator <a href="#upgrade-to-a-validator" id="upgrade-to-a-validator"></a>

To upgrade the node to a validator, you will need to submit a `create-validator` transaction:

```
migalood tx staking create-validator \
  --amount 9000000uwhale \
  --commission-max-change-rate "0.1" \
  --commission-max-rate "0.20" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --details "validators write bios too" \
  --pubkey=$(migalood tendermint show-validator) \
  --moniker $MONIKER_NAME \
  --chain-id $CHAIN_ID \
  --gas-prices 0uwhale \
  --from <key-name>
```

### Backup critical files <a href="#backup-critical-files" id="backup-critical-files"></a>

There are certain files that you need to backup to be able to restore your validator if, for some reason, it is damaged or lost in some way. Please make a secure backup of the following files located in `~/.migalood/config/`:

* `priv_validator_key.json`
* `node_key.json`

It is recommended that you encrypt the backup of these files.
