---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Useful CLI commands

## General <a href="#general" id="general"></a>

#### **Set the default chain for migalood to use:**

```bash
migalood config chain-id <chain-id>
```

{% hint style="info" %}
Mainnet chain ID: `migaloo-1` Testnet chain ID: `narwhal-1`
{% endhint %}

#### Set the default RPC for migalood to use:

```bash
migalood config node <rpc-endpoint>
```

## Queries <a href="#queries" id="queries"></a>

#### Get native/IBC token balances:

```bash
migalood q bank balances <wallet/contract-address> --output json
```

#### Query contract:

```bash
migalood q wasm contract-state smart <contract-address> <query> --output json
```

{% hint style="info" %}
The query needs to be a JSON object, e.g. `{"balance": {"address": "migalood1..."}}`
{% endhint %}

#### Get contract state:

```bash
migalood q wasm contract-state all <contract-address> --output json
```

#### Get contract metadata:

```bash
migalood q wasm contract <contract-address> --output json
```

## Transactions <a href="#transactions" id="transactions"></a>

#### **Upload a wasm binary on chain:**

```bash
migalood tx wasm store <wasm_file> --from <from> --chain-id $CHAIN_ID --node $RPC
```

{% hint style="info" %}
Uploading a wasm binary will return a code\_id, which you can use to instantiate the contract in a subsequent step.
{% endhint %}

#### **Instantiate a contract:**

```bash
migalood tx wasm instantiate <code-id> <instantiate_msg> --label "Contract label" --from <from> --chain-id $CHAIN_ID --node $RPC
```

{% hint style="info" %}
The instantiate\_msg needs to be a JSON object, e.g. `{"owner": "migalood1..."}`
{% endhint %}

#### **Execute a command on a wasm contract:**

```bash
migalood tx wasm execute <contract_addr> <message> --from <from> --amount <coins,optional> --chain-id $CHAIN_ID --node $RPC
```

#### **Send native/IBC tokens:**

```bash
migalood tx bank send <from> <to> <amount> --chain-id $CHAIN_ID --node $RPC --from <from>
```

## Keys

#### **See keys on the current box:**

```bash
migalood keys list
```

#### **Add a key:**

```bash
migalood keys add <name>
```

{% hint style="info" %}
The mnemonic of your key will be displayed on screen only once. Write it down and keep it safe.
{% endhint %}

#### **Import a key from a mnemonic:**

```bash
migalood keys add <name> --recover
```

#### **Delete a key:**

```bash
migalood keys delete <name>
```

## Staking <a href="#staking" id="staking"></a>

#### **Create validator:**

```bash
migalood tx staking create-validator \
--amount 1000000uwhale \
--commission-max-change-rate "0.05" \
--commission-max-rate "0.10" \
--commission-rate "0.05" \
--min-self-delegation "1" \
--pubkey=$(migalood tendermint show-validator) \
--moniker 'Moby Dick' \
--website "https://migaloo.zone" \
--identity "496BD02G58A7E1O9" \
--details "Validator description." \
--security-contact="email@validator.com" \
--chain-id $CHAIN_ID \
--node $RPC  \
--from KEY
```

#### **Delegate tokens:**

```bash
migalood tx staking delegate <validator> <amount> --from <from> --chain-id $CHAIN_ID --node $RPC
```

## Governance <a href="#governance" id="governance"></a>

#### **Query gov proposal:**

```bash
migalood q gov proposal <proposal-id> --chain-id $CHAIN_ID --node $RPC --output json | jq 
```

#### **Vote on gov proposal:**

```bash
migalood tx gov vote <proposal-id> <vote_option> --from <from> --chain-id $CHAIN_ID --node $RPC
```

{% hint style="info" %}
Vote options are: `yes`, `abstain`, `no`, `no_with_veto`
{% endhint %}

## Validators and Nodes <a href="#validators-and-nodes" id="validators-and-nodes"></a>

#### **Check the status of your node:**

```bash
migalood status
```

#### **Get your node ID:**

```bash
migalood tendermint show-node-id
```

#### **Check if your node is catching up:**

```bash
# Query via the RPC (default port: 26657)
curl http://localhost:26657/status | jq .result.sync_info.catching_up
```

{% hint style="info" %}
Your peer address will be the result of this plus host and port, i.e. @:26656 if you are using the default port.
{% endhint %}

#### **Get your valoper address:**

```bash
migalood keys show <your-key-name> -a --bech val
```
