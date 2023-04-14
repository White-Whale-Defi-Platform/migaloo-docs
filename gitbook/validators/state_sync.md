# Sync with State-Sync

The Cosmos SDK includes a feature called State-sync which facilitates rapid connection for validators to the network. By
syncing with a snapshot-enabled RPC from a trusted block height, the time required to join the network is reduced from
several days to mere minutes.

However, the drawback of this approach is that the node will only have access to the most recent state stored by the
state-sync RPC and not the complete transaction history. On the positive side, the database
size is much smaller compared to a fully synced node, thus using state-sync can help reduce operating costs by
minimizing storage usage. Furthermore, when syncing with the network using state-sync, a node can bypass the need for
upgrading procedures and simply sync with the most recent binary version.

{% hint style="info" %}
For nodes that are intended to serve data for dapps, explorers or any other RPC requiring full history, state-syncing to
the network would not be appropriate.
{% endhint %}

## Mainnet state-sync

State-sync with [Polkachu](https://polkachu.com/state_sync/migaloo)
