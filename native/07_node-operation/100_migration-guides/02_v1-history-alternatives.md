---
title: "V1 History Alternatives"
sidebar_position: 1
---

The latest Vaulta v3.1 release officially ends support for the legacy V1 History plugin. Therefore, block producers and node operators who have integrations that rely on V1 History must seek alternative solutions.

## Production Ready Alternatives

The following battle tested and V1 compliant history solutions are available:
- Hyperion History Solution
- Roborovski History API

# Roborovski History API

## Overview

Roborovski History API is designed as a drop-in replacement for the V1 history API. It relies on the Trace API Plugin to extract the history data and then packs it in V1 format before it gives it back to the client request.

## Who Runs Roborovski History API

Roborovski History API is implemented and ran by [Greymass Inc.](https://greymass.com/)

## What makes the Roborovski History API safe

Roborovski History API has a high degree of safety because it is created by [Greymass Inc.](https://greymass.com/) which has been a credible and stable block producer and wallet developer (Anchor) company for Vaulta.

## Understanding the risks associated with hosted solutions

If you rely on a hosted solution, you are reliant on the correctness of data and processes that you do not control. Therefore, if your application critically relies on on-chain data, it is highly recommended that you host your own history solution. However, since Roborovsky is currently closed source, if you want to run your own node you will need to see Hyperion below.

## Roborovski History API and V1 History Standard

Roborovski History API is compliant with the V1 history API standard. It also adds two more functions on top of the standard ones.

Existing V1 History Plugin integrators can simply replace their current API url with Greymass' one and it will work flawlessly.

## API Reference

### How To Connect

The Roborovski History API connection endpoint is `https://vaulta.greymass.com`

### Functions List

- Get Actions (V1 compatible)
    - POST `https://vaulta.greymass.com/v1/history/get_actions`
- Get Transaction (V1 compatible)
    - POST `https://vaulta.greymass.com/v1/history/get_transaction`
- Get Transaction (new method, not in V1)
    - GET `https://vaulta.greymass.com/v1/history/get_transaction?id=<TXID>`
- Get Actions (new method, not in V1)
    - GET `https://vaulta.greymass.com/v1/history/get_actions?account_name=<NAME>`

### Performance Numbers

As it was observed and measured so far the Roborovski History API supports at least 50 requests per second; this limit is defined as a low load, the solution is capable of handling more, but no higher specific limits are known at the moment.



# Hyperion History Solution

## Overview

Hyperion History is a full history solution for indexing, storing and retrieving Vaulta-based blockchain historical data. It can be deployed by node operators to provide data querying support for actions, transactions, and blocks stored on the blockchain.

Hyperion History API provides both V2 and V1 (legacy history plugin) endpoints. Therefore, it is fully compliant with V1 history.

## What makes the Hyperion safe

Hyperion is developed and maintained by EOS Rio: https://eosrio.io/hyperion/ and has been battle tested for almost a decade.

* Github: https://github.com/eosrio/Hyperion-History-API
* Documentation: https://hyperion.docs.eosrio.io/

## Installation

Head over to the [Hyperion Documentation](https://hyperion.docs.eosrio.io/) for installation instructions.


# Memento History Solution

[Memento](https://github.com/Antelope-Memento/antelope_memento) is a blockchain history solution developed by [cc32d9](https://github.com/cc32d9) and [EOS Amsterdam block producer](https://eosamsterdam.net/).

It consists of [Chronicle](https://github.com/EOSChronicleProject/eos-chronicle) and a database writer which stores the transaction traces in a MySQL or Postgres database. Chronicle can be configured to export only transactions relevant to specific accounts, so that the history database does not take too much space.

Two types of [HTTP API](https://github.com/Antelope-Memento/antelope_memento_api) are available: RESTful API (not compatible with v1 or Hyperion), and a GraphQL API.

A [public demo](https://github.com/Antelope-Memento/antelope_memento/blob/main/MEMENTO_PUBLIC_ACCESS.md) is available with 48 hours of history for several public blockchains.
