# Vesting on Migaloo 1

Migaloo 1 features a chain-level vesting account system, commonly used for team and other vesting accounts. However, there is a need for a vesting system tailored to on-chain grant recipients. These recipients cannot be included as normal vesting accounts, as these must be provided either at genesis or during a chain upgrade, which is not feasible for paying out grantees. Instead, we deploy a CosmWasm implementation of vesting with additional features to the spec.

In addition to the ability to create Delayed, Continuous, and Periodic vesting account structures, the admin has the power to claw back these vestings. This option is provided in case community consensus determines that a grantee should have their further vesting grant clawed back. In the event of a clawback, all tokens vested up to the moment the clawback is executed are still available for the grantee to claim, but no further amounts will be provided.

## Native Vesting Code ID

| Label                          | Code ID | Admin                                 | Deployment                                                                                                                                                     |
| ------------------------------ | ------- | ------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| MigalooNativeClawbackableVesting | 21      | migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e | migalood tx wasm execute migaloo15l9a6jpc86dkh366vpqn588pjuhvh0k87kwgmsqp2hsp349w0hvqguj5aw |

### CLI command to instantiate

```json
migalood tx wasm execute migaloo15l9a6jpc86dkh366vpqn588pjuhvh0k87kwgmsqp2hsp349w0hvqguj5aw '{"register_vesting_accounts":{"vesting_accounts":[{"address":"migaloo179mrmjfyqj5zcp8t90tnccjds4rnm3tk0jul4e","schedules":[{"start_point":{"time":1683727249,"amount":"0"},"end_point":{"time":1683734436,"amount":"1000000"}}],"clawbackable":true}]}}' --amount 1000000uwhale
```

The above command demonstrates the creation of a vesting account for an address that will vest 1 WHALE over 2 hours. All times are in epoch timestamp, and all amounts are U128, which means they must be represented as strings in JSON.

## How to instantiate a new vesting account

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