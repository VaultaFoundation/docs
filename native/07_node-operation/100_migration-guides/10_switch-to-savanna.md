---
title: Switching Over To Savanna Consensus Algorithm
---

## Switching Over To Savanna Consensus Algorithm
Switching over to the Savanna Consensus Algorithm is a multi-step process.

### Overview of Upgrade Process
There are four steps
1. Upgrade Antelope Software, Spring and Vaulta System Contracts
2. Block Producers generate and register finality keys
   - First activate protocol feature `BLS_PRIMITIVES2`
   - See section below on [Generate and Registering Finalizer Keys](#generate-and-registering-finalizer-keys)
3. Activate required protocol features
   - Activate `SAVANNA` protocol feature
4. `eosio` user calls `switchtosvnn` action

### Antelope Software Requirements
Switching to Savanna will required the latest version of Spring Software and the Vaulta System Contracts.
- [Spring v1.0.0](https://github.com/AntelopeIO/spring/releases)
- [Vaulta System Contracts v3.6.0](https://github.com/vaultafoundation/system-contracts/releases)

**Note:** [CDT v4.1.0](https://github.com/AntelopeIO/cdt/releases) is needed to compile the latest Vaulta System Contracts. This version of CDT contains both the needed host functions, and cryptography support needed to support managing finalizer keys.

### Protocol Features Dependencies
The reference for protocol features with their corresponding hashes may be found in [bios-boot-tutorial](https://github.com/AntelopeIO/spring/blob/main/tutorials/bios-boot-tutorial/bios-boot-tutorial.py). Switching to the `SAVANNA` consensus algorithm, requires the activation of the `SAVANNA` protocol feature. This feature depends all protocol features available in Spring v1.0.0 being in the active_schedule. This full list of protocol features in Spring v1.0.0 is listed below.
- DISABLE_DEFERRED_TRXS_STAGE_1
- DISABLE_DEFERRED_TRXS_STAGE_2
- WTMSIG_BLOCK_SIGNATURES
- BLS_PRIMITIVES2
- DISALLOW_EMPTY_PRODUCER_SCHEDULE
- ACTION_RETURN_VALUE
- ONLY_LINK_TO_EXISTING_PERMISSION
- FORWARD_SETCODE
- GET_BLOCK_NUM
- REPLACE_DEFERRED
- NO_DUPLICATE_DEFERRED_ID
- RAM_RESTRICTIONS
- WEBAUTHN_KEY
- BLOCKCHAIN_PARAMETERS
- CRYPTO_PRIMITIVES
- ONLY_BILL_FIRST_AUTHORIZER
- RESTRICT_ACTION_TO_SELF
- GET_CODE_HASH
- CONFIGURABLE_WASM_LIMITS2
- FIX_LINKAUTH_RESTRICTION
- GET_SENDER
- SAVANNA

### Generate and Registering Finalizer Keys
The Savanna Consensus algorithm utilized by Spring v1 separates the roles of publishing blocks from signing and finalizing blocks. Finalizer Keys are needed to sign and finalize blocks. In Spring v1, all block producers are expected to be finalizers. There are three steps to creating finalizer keys
- generate your key(s) using `spring-utils`
- add `signature-provider` to configuration with the generated key(s)
- restart nodeos with the new `signature-provider` config
- register a single key on chain with the `regfinkey` action

Additional information on Finalizer Keys may be found in [Guide to Managing Finalizer Keys](../../advanced-topics/managing-finalizer-keys) and [Introduction to Finalizers and Voting](../../advanced-topics/introduction-finalizers-voting).

### Confirmation of Consensus Algorithm
The action `switchtosvnn`, initiates the change to the Savanna Consensus Algorithm, and must be called by the owner of the system contracts. On Vaulta Mainnet this would be the `eosio` user. This is event is called only once per chain.
