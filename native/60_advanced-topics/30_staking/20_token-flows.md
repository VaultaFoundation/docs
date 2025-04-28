---
title: Token Flows
---

<head>
    <title>Staking Token Flows</title>
</head>

The inflows to the Vaulta staking protocol travel through a few different contracts, each with their own responsibilities.
We will cover the contracts that are involved in the inflows to the staking protocol, as well as the origins of the funds.

## Tokenomics Change

In May of 2024 the Vaulta network underwent a tokenomics change that introduced a new bucket of Vaulta tokens 
designated for staking rewards.

**250 million** Vaulta was set aside in this bucket to be emitted with a halving schedule every 4 years. Meaning that in
the first 4 years 125,000,000 Vaulta will be distributed as staking rewards (31,250,000 Vaulta per year), and in the next 4 years
62,500,000 Vaulta will be distributed as staking rewards (15,625,000 Vaulta per year), and so on.

Inflation for the chain was also shut off during that change, and the various buckets replaced that inflated Vaulta.
This means that instead of inflating the chain at a set rate of X% per year, the chain now distributes a percentage of the
pre-allocated reserved supply to various parts of the network, like staking rewards.

## Flow of Staking Rewards

Every time a block producer claims their rewards, they trigger a chain of events that results in the distribution of 
various token allocations to target accounts. 

![Staking Inflows](/images/diagram_rex_inflows.png)

The block producers claim rewards from the **System Contract**, which then flows Vaulta into their accounts, and 
into the `eosio.saving` account that holds a contract which distributes Vaulta to **Vaulta Labs**, the **Vaulta
Foundation**, and the **Staking Rewards** buckets (these are configured in [the `eosio.saving` contract](https://unicove.com/contract/eosio.saving/tables/config)).

As the rewards flow into the `eosio.saving` account, they are immediately split up according to the distribution 
configurations on that contract, which are set by a 15/21 multisig of the block producers and tallied into 
[the `claimers` table](https://unicove.com/contract/eosio.saving/tables/claimers).

Once funds are available, each entity can claim their rewards by calling the `claim` action, which then sends 
the funds to the account specified in the configuration.

In the case of the **Staking Rewards** bucket, the funds are sent to the `eosio.reward` contract, which sit in the 
account until they are distributed to the various strategies that are configured there by an account calling the 
`distribute` action.

When the `distribute` action is called, the funds aimed at Vaulta Staking are used in the [`donatetorex` action]
(https://github.com/vaultafoundation/eos-system-contracts/blob/8ecd1ac6d312085279cafc9c1a5ade6affc886da/contracts/eosio.system/src/rex.cpp#L389)
back on the System Contract which adds those funds to the REX pools, and then sends the funds to `eosio.rex` for 
accounting purposes.

These pathways and mechanisms allow all participants to clearly see the flow of Vaulta as it makes its way through the 
chain's various contracts and accounts, and ensures that the funds are distributed according to the network's
governance decisions instead of being hardcoded into smart contract logic with low visibility and flexibility.

## Flow of User Tokens

![User token flow](/images/diagram_user_token_flow.png)

When a user stakes their Vaulta to the protocol staking system they are transferring their Vaulta tokens to the system 
contract which then issues them REX tokens in return and forwards their Vaulta to the `eosio.rex` account for accounting 
purposes.

Since the value of REX can never go down, the user is guaranteed to receive _at least_ the same amount of Vaulta back when 
they unstake, sell, and withdraw their REX tokens.