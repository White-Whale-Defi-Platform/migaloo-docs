# Modules



### [Alliance Module](https://docs.alliance.money/)

Alliance is an [open-source Cosmos SDK module](https://github.com/terra-money/alliance) that leverages interchain staking to form economic alliances among blockchains. General information on the Alliance module can be found in the [Overview](https://docs.alliance.money/overview) and [How it works](https://docs.alliance.money/alliance) sections. Visit the [Integration guide](https://docs.alliance.money/guides/get-started) to learn how to add Alliance to your chain. For detailed information on Alliance, visit the [in-depth concepts section](https://docs.alliance.money/concepts/staking) or the [Technical specs](https://docs.alliance.money/tech/parameters).

### [Auth](https://docs.cosmos.network/v0.46/modules/auth/)

The auth module is responsible for specifying the base transaction and account types for an application since the SDK itself is agnostic to these particulars. It contains the middlewares, where all basic transaction validity checks (signatures, nonces, auxiliary fields) are performed, and exposes the account keeper, which allows other modules to read, write, and modify accounts.

### [Authz](https://docs.cosmos.network/v0.46/modules/authz/)

authz is an implementation of a Cosmos SDK module that allows granting arbitrary privileges from one account (the granter) to another account (the grantee). Authorizations must be granted for a particular Msg service method one by one using an implementation of the `Authorization` interface.

### [Bank](https://docs.cosmos.network/v0.46/modules/bank/)

The bank module is responsible for handling multi-asset coin transfers between accounts and tracking special-case pseudo-transfers which must work differently with particular kinds of accounts (notably delegating/undelegating for vesting accounts). It exposes several interfaces with varying capabilities for secure interaction with other modules which must alter user balances.\
In addition, the bank module tracks and provides query support for the total supply of all assets used in the application.

### [Capability](https://docs.cosmos.network/v0.46/modules/capability/)

Capability is an implementation of a Cosmos SDK module that allows for provisioning, tracking, and authenticating multi-owner capabilities at runtime.

### [Crisis](https://docs.cosmos.network/v0.46/modules/crisis/)

The crisis module halts the blockchain under the circumstance that a blockchain invariant is broken. Invariants can be registered with the application during the application initialization process.

### [Distribution](https://docs.cosmos.network/v0.46/modules/distribution/)

This _simple_ distribution mechanism describes a functional way to passively distribute rewards between validators and delegators. Note that this mechanism does not distribute funds in as precisely as active reward distribution mechanisms and will therefore be upgraded in the future.

### [Evidence](https://docs.cosmos.network/v0.46/modules/evidence/)

Evidence is an implementation of a Cosmos SDK module that allows for the submission and handling of arbitrary evidence of misbehaviour such as equivocation and counterfactual signing.

### [Fee Burn](https://github.com/White-Whale-Defi-Platform/migaloo-chain/tree/release/v4.1.x/x/feeburn)

A custom built Cosmos SDK module allowing for the burning of transaction fees. Fee burn percentage can be set anywhere between 0% and 100%

### [Fee grant](https://docs.cosmos.network/v0.46/modules/feegrant/)

This module allows accounts to grant fee allowances and to use fees from their accounts. Grantees can execute any transaction without the need to maintain sufficient fees.

### [Genutil](https://docs.cosmos.network/main/build/modules/genutil)

The `genutil` package contains a variety of genesis utility functionalities for usage within a blockchain application.

### [Gov](https://docs.cosmos.network/v0.46/modules/gov/)

The module enables Cosmos-SDK based blockchain to support an on-chain governance system. In this system, holders of the native staking token of the chain can vote on proposals on a 1 token 1 vote basis. Next is a list of features the module currently supports:

### IBC

A protocol for secure and reliable communication between heterogeneous blockchains built on the Cosmos SDK. IBC enables the transfer of tokens and data across multiple blockchains.

### IBC hooks

The wasm hook is an IBC middleware which is used to allow ICS-20 token transfers to initiate contract calls. This allows cross-chain contract calls, that involve token movement. This is useful for a variety of use cases. One of primary importance is cross-chain swaps, which is an extremely powerful primitive.

### ICA

Interchain Accounts is the Cosmos SDK implementation of the ICS-27 protocol, which enables cross-chain account management built upon IBC.

### ICQ

Interchain Queries (ICQ) are a feature that allows a blockchain to read data from another network, allowing developers to securely retrieve data from its storage by requesting information about the network or application state.

### [Mint](https://docs.cosmos.network/v0.46/modules/mint/)

The mint module is in charge of the creation of new WHALE through minting. At the beginning of every block, new WHALE is released by the Mint module and sent to the fee collector account to be distributed to stakers as rewards.\
\
Migaloo inflation is set at 4%

### [Params](https://docs.cosmos.network/v0.46/modules/params/)

Package params provide a globally available parameter store.

### Packet Forwarding Middleware (PFM)

The packet-forward-middleware is an IBC middleware module built for Cosmos blockchains utilizing the IBC protocol. A chain which incorporates the packet-forward-middleware is able to route incoming IBC packets from a source chain to a destination chain.

### [Slashing](https://docs.cosmos.network/v0.46/modules/slashing/)

The slashing module enables Cosmos SDK-based blockchains to disincentivize any attributable action by a protocol-recognized actor with value at stake by penalizing them ("slashing").

### [Staking](https://docs.cosmos.network/v0.46/modules/staking/)

The module enables Cosmos SDK-based blockchain to support an advanced Proof-of-Stake (PoS) system. In this system, holders of the native staking token of the chain can become validators and can delegate tokens to validators, ultimately determining the effective validator set for the system.

### [Token factory](https://docs.osmosis.zone/osmosis-core/modules/tokenfactory/)

The tokenfactory module allows any account to create a new token with the name `factory/{creator address}/{subdenom}`. Because tokens are namespaced by creator address, this allows token minting to be permissionless, due to not needing to resolve name collisions. A single account can create multiple denoms, by providing a unique subdenom for each created denom. Once a denom is created, the original creator is given "admin" privileges over the asset.

### Transfer

Transfer is the Cosmos SDK implementation of the [ICS-20](https://github.com/cosmos/ibc/tree/master/spec/app/ics-020-fungible-token-transfer) protocol, which enables cross-chain fungible token transfers.

### [Upgrade](https://docs.cosmos.network/v0.46/modules/upgrade/)

Upgrade is an implementation of a Cosmos SDK module that facilitates smoothly upgrading a live Cosmos chain to a new (breaking) software version.

### Wasm

The Wasm module implements the execution environment for WebAssembly smart contracts, powered by [CosmWasm](https://cosmwasm.com/).\
