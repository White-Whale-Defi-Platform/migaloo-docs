# Alliance Hub Migaloo

- [Alliance Hub Migaloo](#alliance-hub-migaloo)
	- [Overview](#overview)
	- [Deployments and associated information](#deployments-and-associated-information)
	- [Usage](#usage)
		- [Intent](#intent)
			- [For Holders](#for-holders)
			- [Contract methods](#contract-methods)
				- [Instantiate](#instantiate)
		- [Running from CLI](#running-from-cli)
				- [Reward Distribution](#reward-distribution)
				- [Updating rewards](#updating-rewards)
			- [User facing methods](#user-facing-methods)
				- [Claiming rewards](#claiming-rewards)


## Overview

Alliance is an open-source Cosmos SDK module that leverages interchain staking to form economic alliances among blockchains.

🛠 How Alliance works
Alliance is an [open-source Cosmos SDK module](https://github.com/terra-money/alliance) that leverages interchain staking to form economic alliances among blockchains. The following section is a general outline of the Alliance module. For a detailed overview of how Alliance staking works, visit the [in-depth concepts section](https://docs.alliance.money/concepts/staking).

For the most up-to-date references to Alliance head to Terra's [Alliance Documentation](https://docs.alliance.money/)

This doc will be focused on the Migaloo-based fork which adds support for CW20s and other customizations as well as removing the Oracle based system and replacing it with a more simple Gauge of distribution percentages. This design may be improved at a later point to use Gauges for the reallocation of distributions based on some criteria. For example we could have a number of LP tokens which over time have their weights adjusted either by governance votes or by some other mechanism for example the amount of volume they are providing to the network or the fees. The first system mentioned which includes a Gauge is closely modelled after Curves Gauges system and the second could more akin to Terra's implementation of Alliance with the Oracle system. 

Where to use this contract: [Migaloo Zone](https://app.migaloo.zone/)

## Deployments and associated information

| Label                            | Code ID | Admin                                          | Deployment                                                                                  |
| -------------------------------- | ------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------- |
| MigalooAllianceEcosystemAccelerator | 152      | migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd | migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd |

## Usage 

### Intent

The Migaloo Alliance module is intended to be used to stimulate the economies of projects by redirecting some inflation to holders who stake their token based on configurable weighting.

#### For Holders
Stake idle CW20s for redirected inflation through Alliance
Stake unbonded LP tokens for redirected inflation through Alliance

#### For Chain
Stimulate the economies of projects by redirecting some inflation to holders who stake their token based on configurable weighting.

#### Contract methods 

##### Instantiate 
When instantiating a number of actor addrs must be provided for the governance, the controller and the oracle. These can all be set to the same address if desired but its split to allow better role based access

```mermaid
graph TD
A(Asset Whitelisted) --> B(Holders of Asset can Stake)
B --> C(Holders stake to contract)
C --> D(Rewards Updated)
D --> E(Stakers claim rewards for staked asset)
```



### Running from CLI 


#### Examples 
##### Whitelisting a CW20 

```json
migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd '{ "whitelist_assets": {"migaloo-1": [{ "cw20": "migaloo1xr3rq8yvd7qplsw5yx90ftsr2zdhg4e9z60h5duusgxpv72hud3s54xttx"}]}}'
```

##### Whitelisting a Native 

```json
migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd '{ "whitelist_assets": {"migaloo-1": [{ "native": "factory/migaloo1xr3rq8yvd7qplsw5yx90ftsr2zdhg4e9z60h5duusgxpv72hud3s54xttx/uLP"}]}}'
```

##### Whitelisting a CW721 (NFT) 
This one is experimental and not yet supported. 

```json
migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd '{ "whitelist_assets": {"migaloo-1": [{ "cw721": "migaloo1xr3rq8yvd7qplsw5yx90ftsr2zdhg4e9z60h5duusgxpv72hud3s54xttx"}]}}'
```

After which any holders of that asset can now stake and unstake. Rewards will only be accumulated if the asset is also included in the reward_distribution.

##### Reward Distribution 
The Reward Distribution represents a vector of assets and their respective weights for the sharing of the redirected inflation 
The weights must add to 100% when being set. 

Important to note reward distribution is separate from whitelisting for a reason. This enables assets to be whitelisted before any rewards are shared and additionally ensures rewards are independent of the whitelist.

Also important to note setting new reward distributions is a one-hot operation that needs to be done with the entire list of distribution. 
This removes any recalculation logic from the contract and enforces that the governance/admin actor must provide any updated rates and that despite updates they all add to 100%. 

Example: 
3 assets are whitelisted and a reward distribution is set at 33% for each. 
```mermaid

pie title ASSET_REWARD_DISTRIBUTION 
		"Fable" : 33
		"WHALE-USDC LP" : 33
		"WHALE-BTC LP" : 33
```




At some point in the future this reward distribution can be re-weighted 30,20,40 without touching the whitelist 
```mermaid

pie title ASSET_REWARD_DISTRIBUTION 
		"Fable" : 30
		"WHALE-USDC LP" : 30
		"WHALE-BTC LP" : 40
```


A fourth asset is whitelist and the reward distribution is set again: 

```bash
./migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd '{"set_asset_reward_distribution": [{"asset": {"native": "factory/addr/fable"}, "distribution": "0.25"}, {"asset":{"cw20":"migaloo1xr3rq8yvd7qplsw5yx90ftsr2zdhg4e9z60h5duusgxpv72hud3s54xttx"}, "distribution": "0.3"}, {"asset": {"cw20":"migaloo1anotherone"}, "distribution": "0.4"}, {"asset": {"native": "factory/migaloo1v767q4apajgksqlg5ejdakn8auszecje3yqfw6/fable"}, "distribution": "0.05"}]}'
```

The rewards for all assets have been updated at once
```mermaid

pie title ASSET_REWARD_DISTRIBUTION 
		"Fable" : 25
		"WHALE-USDC LP" : 30
		"WHALE-BTC LP" : 40
		"anotherone": 5
```


##### Updating rewards
Updating rewards is the process in which all earned rewards from the redirect inflation is tallied by querying the balance of the reward denom on the contract. 

For each validator in the set VALIDATORS state item, an Alliance `MsgClaimDelegationRewards` is prepared to be sent as well as a UpdateRewardsCallback for the contract where newly received rewards will be allocated. 

In the Callback, the amount of newly gained assets is tallied to determine the rewards_collected. 
These rewards_collected are then allocated based on the assets distribution percent.
If there are not balances for a given asset, no rate update happens which also means no emissions are redirected to them. 

The above actions set an `ASSET_REWARD_RATE` for each asset which is then used when staking, unstaking or claiming via the `_claim_rewards`. All unclaimed rewards are gathered on stake, unstake and then claimed whenever claim_rewards is called.

#### User facing methods

##### Claiming rewards 
Rewards claimable by a staker of a whitelisted asset can claim their earned rewards after the needed steps in [[Alliance Hub Migaloo#Updating Rewards]] have been performed. This should be done on some interval meaning rewards are being accumulated steadily. 

Users in this scenario can claim their rewards one asset per time. 

The asset that must be passed is an alias of `AssetInfoBase<Addr>` from cw-asset. It looks like so 

```rust
pub type AssetInfo = AssetInfoBase<Addr>;

#[cw_serde]
#[non_exhaustive]
pub enum AssetInfoBase<T> {
	Native(String),
	Cw20(T),
	Cw1155(T, String),
}
```
The claiming message for a given asset may look like: 

```bash
./migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd '{"claim_rewards": {"cw20": "migaloo1xr3rq8yvd7qplsw5yx90ftsr2zdhg4e9z60h5duusgxpv72hud3s54xttx"}}' --from new_deploy_wallet --gas auto --gas-adjustment 1.4

```
In the case of a CW20 token or 
```bash
./migalood tx wasm execute migaloo190qz7q5fu4079svf890h4h3f8u46ty6cxnlt78eh486k9qm995hquuv9kd '{"claim_rewards": {"native": "factory/migaloo1v767q4apajgksqlg5ejdakn8auszecje3yqfw6/fable"}}' --from new_deploy_wallet --gas auto --gas-adjustment 1.4
```

In the case of a native token

Provided the needed steps for Updating Rewards have been performed. All due rewards will be claimed for the requested asset.
This operation must be repeated for each different asset you may have staked. 