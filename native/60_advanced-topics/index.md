---
title: Advanced Topics
sidebar_class_name: sidebarhidden
---


## Core

The `Vaulta Core` provides the basic building blocks for the `system` layer. However, since they are not implemented as smart contracts, they do not provide the same level of flexibility. Nevertheless, the `core` implementation is also open source, and thus it can be modified to suit custom business requirements.

The core protocols are:

1. [Transactions Protocol](02_transactions-protocol.md)

## System

The Vaulta blockchain is unique in that the features and characteristics of the blockchain built on it are flexible, that is, they can be changed or be modified completely to suit each business case requirement. Core blockchain features such as consensus, fee schedules, account creation and modification, token economics, block producer registration, voting, multi-sig, etc., are implemented inside smart contracts which are deployed on the blockchain built on the Vaulta blockchain. These smart contracts are referred to as `system contracts` and the layer as the `Vaulta system` layer, or simply the `system` layer.

The Vaulta Network Foundation implements and maintains these `system contracts` as reference implementations only, encapsulating the base functionality for an Vaulta-based blockchain. The `system contracts` are listed below:

* [eosio.bios](https://github.com/vaultafoundation/system-contracts/tree/main/contracts/eosio.bios) - The `eosio.bios` contract is a special contract that is used to initialize the blockchain
* [eosio.system](https://github.com/vaultafoundation/system-contracts/tree/main/contracts/eosio.system) - The `eosio.system` contract is the core contract that implements the foundational Vaulta blockchain features
* [eosio.msig](https://github.com/vaultafoundation/system-contracts/tree/main/contracts/eosio.msig) - The `eosio.msig` contract implements the multi-signature functionality
* [eosio.token](https://github.com/vaultafoundation/system-contracts/tree/main/contracts/eosio.token) - The `eosio.token` contract implements the system token functionality
* [eosio.wrap](https://github.com/vaultafoundation/system-contracts/tree/main/contracts/eosio.wrap) - The `eosio.wrap` contract implements a governance feature
