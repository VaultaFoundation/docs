---
title: producer_plugin
dont_translate_title: true
---

## Overview

The `producer_plugin` contains functionality for a node to perform the task of block production. It also implements the core functionality provided by the [Producer API plugin](./producer-api-plugin.md).

> ℹ️ To enable block production, a particular `nodeos` configuration is necessary. Refer to the [Configuring Block Producing Node](https://docs.vaulta.com/manuals/leap/latest/nodeos/usage/node-setups/producing-node) guide for detailed instructions.

## Usage

```console
# config.ini
plugin = eosio::producer_plugin [options]
```
```sh
# nodeos startup params
nodeos ... -- plugin eosio::producer_plugin [options]
```

## Options

These can be specified from either the `nodeos` command-line or the `config.ini` file:

### Config Options for `producer_plugin`

Option (=default) | Description
-|-
`-e [ --enable-stale-production ]` | Enable block production, even if the chain is stale.
`-x [ --pause-on-startup ]` | Start this node in a state where production is paused
`--max-transaction-time arg (=30)` | Limits the maximum time (in milliseconds) that is allowed a pushed transaction's code to execute before being considered invalid
`--max-irreversible-block-age arg (=-1)` | Limits the maximum age (in seconds) of the DPOS Irreversible Block for a chain this node will produce blocks on (use negative value to indicate unlimited)
`-p [ --producer-name ] arg` | ID of producer controlled by this node (e.g. inita; may specify multiple times)
`--signature-provider arg (=<PUBLIC_KEY>=KEY:<PRIVATE_KEY>)` | Key=Value pairs in the form ^public-key^=^provider-spec^ Where: ^public-key^    is a string form of a vaild Vaulta public key ^provider-spec^ is a string in the form ^provider-type^ :^data^ ^provider-type^ is KEY, KEOSD, or SE KEY:^data^      is a string form of a valid Vaulta private key which maps to the provided public key KEOSD:^data^    is the URL where keosd is available and the approptiate wallet(s) are unlocked
`--greylist-account arg` | account that can not access to extended CPU/NET virtual resources
`--greylist-limit arg (=1000)` | Limit (between 1 and 1000) on the multiple that CPU/NET virtual resources can extend during low usage (only enforced subjectively; use 1000 to not enforce any limit)
`--produce-time-offset-us arg (=0)` | Offset of non last block producing time in microseconds. Valid range 0 .. -block_time_interval.
`--last-block-time-offset-us arg (=-200000)` | Offset of last block producing time in microseconds. Valid range 0 .. -block_time_interval.
`--cpu-effort-percent arg (=80)` | Percentage of cpu block production time used to produce block. Whole number percentages, e.g. 80 for 80%
`--last-block-cpu-effort-percent arg (=80)` | Percentage of cpu block production time used to produce last block. Whole number percentages, e.g. 80 for 80%
`--max-block-cpu-usage-threshold-us arg (=5000)` | Threshold of CPU block production to consider block full; when within threshold of max-block-cpu-usage block can be produced immediately
`--max-block-net-usage-threshold-bytes arg (=1024)` | Threshold of NET block production to consider block full; when within threshold of max-block-net-usage block can be produced immediately
`--max-scheduled-transaction-time-per-block-ms arg (=100)` | Maximum wall-clock time, in milliseconds, spent retiring scheduled transactions (and incoming transactions according to incoming-defer-ratio) in any block before returning to normal transaction processing.
`--subjective-cpu-leeway-us arg (=31000)` | Time in microseconds allowed for a transaction that starts with insufficient CPU quota to complete and cover its CPU usage.
`--subjective-account-max-failures arg (=3)` | Sets the maximum amount of failures that are allowed for a given account per window size.
`--subjective-account-max-failures-window-size arg (=1)` | Sets the window size in number of blocks for subjective-account-max-failu res.
`--subjective-account-decay-time-minutes arg (=1440)` | Sets the time to return full subjective cpu for accounts
`--incoming-defer-ratio arg (=1)` | ratio between incoming transactions and deferred transactions when both are queued for execution
`--incoming-transaction-queue-size-mb arg (=1024)` | Maximum size (in MiB) of the incoming transaction queue. Exceeding this value will subjectively drop transaction with resource exhaustion.
`--disable-subjective-billing arg (=1)` | Disable subjective CPU billing for API/P2P transactions
`--disable-subjective-account-billing arg` | Account which is excluded from subjective CPU billing
`--disable-subjective-p2p-billing arg (=1)` | Disable subjective CPU billing for P2P transactions
`--disable-subjective-api-billing arg (=1)` | Disable subjective CPU billing for API transactions
`--producer-threads arg (=2)` | Number of worker threads in producer thread pool
`--snapshots-dir arg (="snapshots")` | the location of the snapshots directory (absolute path or relative to application data dir)
`--read-only-threads arg` | Number of worker threads in read-only execution thread pool. Max 8.
`--read-only-write-window-time-us arg (=200000)` | Time in microseconds the write window lasts.
`--read-only-read-window-time-us arg (=60000)` | Time in microseconds the read window lasts.

## Dependencies

* [`chain_plugin`](./chain-plugin.md)

## The priority of transaction

You can give one of the transaction types priority over another when the producer plugin has a queue of transactions pending.

The option below sets the ratio between the incoming transaction and the deferred transaction:

```console
  --incoming-defer-ratio arg (=1)       
```

By default value of `1`, the `producer` plugin processes one incoming transaction per deferred transaction. When `arg` sets to `10`, the `producer` plugin processes 10 incoming transactions per deferred transaction.

If the `arg` is set to a sufficiently large number, the plugin always processes the incoming transaction first until the queue of the incoming transactions is empty. Respectively, if the `arg` is 0, the `producer` plugin processes the deferred transactions queue first.


### Load Dependency Examples

```console
# config.ini
plugin = eosio::chain_plugin [operations] [options]
```
```sh
# command-line
nodeos ... --plugin eosio::chain_plugin [operations] [options]
```

For details about how blocks are produced please read the following [block producing explainer](https://docs.vaulta.com/manuals/leap/latest/nodeos/plugins/producer_plugin/block-producing-explained).
