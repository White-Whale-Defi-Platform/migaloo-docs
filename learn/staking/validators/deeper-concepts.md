# Deeper Concepts

### What are the responsibilities of a validator?

Validators have two main responsibilities:

* **Be able to constantly run a correct version of the software:** Validators must ensure that their servers are always online and their private keys are not compromised.
* **Actively participate in governance:** Validators are required to vote on proposals.

Additionally, validators are expected to be active members of the community. Validators must always be up-to-date with the current state of the ecosystem so that they can easily adapt to any change.

### What does 'participate in governance' entail? <a href="#what-does-participate-in-governance-entail" id="what-does-participate-in-governance-entail"></a>

Validators and delegators on Migaloo Zone can vote on proposals to change operational parameters (such as the block gas limit), coordinate upgrades, or make a decision on any given matter.

Validators play a special role in the governance system. As pillars of the system, validators are required to vote on behalf of their delegators. It is especially important since delegators who do not vote inherit the vote of their validator, for this reason, delegators are encouraged to choose a validator that aligns with their own views.

### How does a Validator enter the 'Active Set'?

Migaloo Zone has a validator set of 50. To become an active validator on Migaloo a validator must receive more delegations than the bottom validator. This is not a set number in terms of WHALE or dollar amount and can vary. To enter the active set anyone can delegate to the validator, including the Validator themselves.

### Can a validator run away with their delegators' WHALE? <a href="#can-a-validator-run-away-with-their-delegators-atom" id="can-a-validator-run-away-with-their-delegators-atom"></a>

By delegating to a validator, a user delegates voting power. The more voting power a validator have, the more weight they have in the consensus and governance processes. This does not mean that the validator has custody of their delegators' WHALE. **A validator cannot run away with its delegator's funds**.

Even though delegated funds cannot be stolen by their validators, delegators' tokens can still be slashed by a small percentage if their validator suffers a [slashing event](deeper-concepts.md#what-are-the-slashing-conditions), which is why we encourage due diligence when [selecting a validator](choose-a-validator.md).

### How often is a validator chosen to propose the next block? Does frequency increase with the quantity of bonded WHALE? <a href="#how-often-is-a-validator-chosen-to-propose-the-next-block-does-frequency-increase-with-the-quantity" id="how-often-is-a-validator-chosen-to-propose-the-next-block-does-frequency-increase-with-the-quantity"></a>

The validator that is selected to propose the next block is called the proposer. Each proposer is selected deterministically. The frequency of being chosen is proportional to the voting power (i.e. amount of bonded WHALE) of the validator.

### What is the incentive to run a validator?[​](https://hub.cosmos.network/validators/validator-faq#what-is-the-incentive-to-run-a-validator) <a href="#what-is-the-incentive-to-run-a-validator" id="what-is-the-incentive-to-run-a-validator"></a>

Validators earn proportionally more revenue than their delegators because of the commission they take on the staking rewards from their delegators.

Validators also play a major role in governance. If a delegator does not vote, they inherit the vote from their validator. This voting inheritance gives validators a major responsibility in the ecosystem.

### What are the slashing conditions?[​](https://hub.cosmos.network/validators/validator-faq#what-are-the-slashing-conditions) <a href="#what-are-the-slashing-conditions" id="what-are-the-slashing-conditions"></a>

If a validator misbehaves, their delegated stake is partially slashed. Two faults can result in slashing of funds for a validator and their delegators:

* **Double signing:** If someone reports on chain A that a validator signed two blocks at the same height on chain A and chain B, and if chain A and chain B share a common ancestor, then this validator gets slashed by 5% on chain A.
* **Downtime:** If a validator misses more than `95%` of the last `10,000` blocks (roughly \~15.5 hours), they are slashed by 0.01%.

