---
description: >-
  'Vanilla' Staking refers to the act of delegating your WHALE coins to a
  validator to secure the chain and participate in governance.
---

# Vanilla Staking

## Staking

The Migaloo Zone consists of 50 active [validators.md](validators.md "mention") who work to participate in consensus, validate transactions and produce the chains 'blocks'. Validators are assigned a rank based on the number of tokens delegated to them, the higher a validator's rank the more WHALE has been delegated to them and thus the more voting power they have as 1 WHALE = 1 vote.&#x20;

## Delegating

Migaloo Zone is a Delegated Proof of Stake chain. Users delegate (aka Stake or Bond) their Coins to a validator. Delegating to a validator helps to secure the chain and allows the user's chosen validator to vote on governance proposals on the stakers behalf. Stakers can override their validator's decision should they disagree by manually voting themselves. Once a user has staked to a validator in the active set they start earning WHALE rewards from the chain's inflation and transaction fees. Migaloo stakers also earn other coins such as wBTC and OSMO LSTs. To learn more about this please read [restaking](restaking/ "mention")

## Undelegating and Redelegating

Users are able to undelegate their staked coins at any time. Once a user has confirmed their undelegation transaction their coins enter into the "undelegation period". This period lasts 21 days (typical for a Cosmos Chain), after the 21-day period the user's coins will appear back in their wallet.

Once a user has initiated the undelegation process it can not be cancelled. Users must wait the full 21-day period for their coins to become liquid again. During undelegations, users do not receive any staking rewards nor are the tokens tradable.

Redelegating instantly stakes a user's WHALE to a different validator than the one they are currently delegated to. Once a user has redelegated they are unable to make a further redelegation of those coins until a 21-day period has elapsed.

## Staking Rewards

Staking on Migaloo is incentivised by rewarding stakers and validators with WHALE inflation (4%) and a portion of the chain's transaction fees. These rewards are distributed to stakers and validators at the end of each block (5.7 seconds). These rewards are distributed to delegators based on the amount of coins staked. Validator's rewards come from the commission they charge in order to run their services.

## Slashing

Slashing is an event, which results in a loss of stake percentage when a validator misbehaves. It is a financial incentive to act properly and also a measure to prevent a 'nothing at stake' problem. When a validator fails to sign too many pre-commits or causes a double-sign, slashing can happen. When a validator gets slashed, their delegators’ WHALE will be slashed as well.&#x20;

Slashing can hurt delegators’ shares, so delegators must be aware of their validators’ past records such as downtime to pick the safest and most secure validator.
