# Setting up Cosmovisor

### What is Cosmovisor?

Cosmovisor is a small process manager for Cosmos SDK application binaries that monitors the governance module for incoming chain upgrade proposals. If it sees a proposal that gets approved, Cosmovisor can automatically download the new binary, stop the current binary, switch from the old binary to the new one, and finally restart the node with the new binary.
