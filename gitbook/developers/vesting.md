# Vesting on Migaloo

<!-- Need a table of all the links or a list  -->





## Overview

Migaloo 1 features a chain-level vesting account system, commonly used for team and other vesting accounts. However, there is a need for a vesting system tailored to on-chain grant recipients. These recipients cannot be included as normal vesting accounts, as these must be provided either at genesis or during a chain upgrade, which is not feasible for paying out grantees. Instead, we deploy a CosmWasm implementation of vesting with additional features to the spec.

In addition to the ability to create Delayed, Continuous, and Periodic vesting account structures, the admin has the power to claw back these vestings. This option is provided in case community consensus determines that a grantee should have their further vesting grant clawed back. In the event of a clawback, all tokens vested up to the moment the clawback is executed are still available for the grantee to claim, but no further amounts will be provided.

## Deployments and associated information

| Label                            | Code ID | Admin                                          | Deployment                                                                                  |
| -------------------------------- | ------- | ---------------------------------------------- | ------------------------------------------------------------------------------------------- |
| MigalooNativeClawbackableVesting | 21      | migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e | migalood tx wasm execute migaloo15l9a6jpc86dkh366vpqn588pjuhvh0k87kwgmsqp2hsp349w0hvqguj5aw |

## Usage 


### Running from CLI 

```json
migalood tx wasm execute migaloo15l9a6jpc86dkh366vpqn588pjuhvh0k87kwgmsqp2hsp349w0hvqguj5aw '{"register_vesting_accounts":{"vesting_accounts":[{"address":"migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e","schedules":[{"start_point":{"time":1683727249,"amount":"0"},"end_point":{"time":1683734436,"amount":"1000000"}}],"clawbackable":true}]}}' --amount 1000000uwhale
```

The above command demonstrates the creation of a vesting account for an address that will vest 1 WHALE over 2 hours. All times are in epoch timestamp, and all amounts are U128, which means they must be represented as strings in JSON.

## Examples 
The following are a few pre-made examples with info describing each one in details. The purpose is to have a quick reference for the creation of future vesting props 
### How to instantiate a new vesting account

To instantiate a new vesting account, follow these steps:

1. Prepare a JSON object containing the vesting account information. The object should include the following properties:
    - `address`: The address of the grant recipient.
    - `schedules`: An array of vesting schedules, a schedule is a series of SchedulePoints each with a `start_point` and `end_point`. Both points should have `time` (epoch timestamp) and `amount` (U128) properties.
    - `clawbackable`: A boolean value indicating whether the vesting is clawbackable.

Example JSON object:

```json
{
  "register_vesting_accounts": {
    "vesting_accounts": [
      {
        "address": "migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e",
        "schedules": [
          {
            "start_point": {
              "time": 1683727249,
              "amount": "0"
            },
            "end_point": {
              "time": 168373
            }
          }]
      }]
  }
}
```

### Vesting account with an amount upfront 

To create a vesting account with an amount upfront, add a `start_point` to the first schedule point with a non-zero amount. The `start_point` should have a `time` property that is equal to or less than the current time, and an `amount` property that starts at zero. The `end_point` should have a `time` property that is equal to the current time or very close to it such that when deployed this first schedule will be immediately available, and an `amount` property that is equal to the total amount to be vested 'upfront'. 

The remainder of the vesting schedule will be like other examples. In this case the vesting account we create has two schedules. One which is the upfront amount and the second which will be the remainder of the vesting schedule less any upfront. Many more changes and variations can be done than just two but this is the common example. 

When creating a vesting account for 5m tokens with 10% upfront we will have 2 'schedules' in the schedules array. The first will be the upfront amount of 500k tokens and the second will be the remainder of the vesting schedule of 4.5m tokens.

> Note: In this example, we are using a token with 6 decimal places, assuming WHALE token and that we are using uwhale. Any tokens can be used but the decimals must be accounted for.
> Inspecting this vesting account you will notice in the second schedule the start_point has an amount of 0.5 and the end_point has an amount of 4.5m in udenom. This is because the first schedule has already vested 500k tokens and the second schedule will at most vest another 4.5m tokens, bringing the total to 5m tokens. 

Example JSON object:

```json
{
  "register_vesting_accounts": {
    "vesting_accounts": [
      {
        "address": "migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e",
        "schedules": [
          {
            "start_point": {
              "time": 1683727249,
              "amount": "0"
            },
            "end_point": {
              "time": 1683734436,
              "amount": "500000000000"
            }
          },
          {
            "start_point": {
              "time": 1683734436,
              "amount": "500000000000"
            },
            "end_point": {
              "time": 1683734436,
              "amount": "4500000000000"
            }
          }]
      }]
  }
}
```

### Vesting account with a Cliff

A cliff is a period of time before which no tokens are vested. To create a vesting account with a cliff, add a `start_point` to the first schedule point with a non-zero amount. The `start_point` should have a `time` property that is equal to the cliff time, and an `amount` property that is equal to zero.
The start_point should be in the future, until this point is hit. Nothing is accrued.
After which it vests linearly until the end_point is hit.

Example JSON object:

Assuming 1683727249 is a time in the future
```json
{
  "register_vesting_accounts": {
    "vesting_accounts": [
      {
        "address": "migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e",
        "schedules": [
          {
            "start_point": {
              "time": 1683727249,
              "amount": "0"
            },
            "end_point": {
              "time": 1683734436,
              "amount": "1000000"
            }
          }]
      }]
  }
}
```


## Claiming from Grants/Vesting Accounts

If a vesting account has be created for you or your team for the purposes of a grant or otherwise you will need to claim the tokens. This is done by calling the `claim` function on the vesting contract. 
This is a major difference to a native cosmos-sdk vesting account in that the tokens do not just end up in the wallet but also there are permissions around the claim such as the ability to clawback the tokens.

To claim the tokens you will need to know the vesting contract address and the recipient address. The recipient address is the address that the vesting account was created for.

### Claiming from CLI 

TODO: Review this example with community and verify its good enough, is more info needed? 
```json

migalood tx wasm execute migaloo15l9a6jpc86dkh366vpqn588pjuhvh0k87kwgmsqp2hsp349w0hvqguj5aw '{"claim":{"recipient":"migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e"}}' --amount 1000000uwhale --from migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e
```

