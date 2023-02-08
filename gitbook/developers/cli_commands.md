# Useful CLI commands

Check the status of your node:

```bash
migalood status
```

Check if your node is catching up:

```bash
# Query via the RPC (default port: 26657)
curl http://localhost:26657/status | jq .result.sync_info.catching_up
```
Get your node ID:

```bash
migalood tendermint show-node-id
```

{% hint style="info" %}
Your peer address will be the result of this plus host and port, i.e. <id>@<host>:26656 if you are using the default
port.
{% endhint %}

Get your valoper address:

```bash
migalood keys show <your-key-name> -a --bech val
```

Check the default chain for migalood to use:

```bash
migalood config chain-id <chain-id>
```

Mainnet chain ID: `migaloo-1`
Testnet chain ID: `narwhal-1`

See keys on the current box:

```bash
migalood keys list
```

Import a key from a mnemonic:

```bash
migalood keys add <name> --recover
```

Get contract state:

```bash 
migalood q wasm contract-state all <contract-address> --output json
```

