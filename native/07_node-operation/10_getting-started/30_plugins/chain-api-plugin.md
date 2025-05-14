---
title: chain_api_plugin
dont_translate_title: true
---

See [Chain API Reference](https://docs.vaulta.com/apis/leap/latest/chain.api/).

## Overview

The `chain_api_plugin` exposes functionality from the [`chain_plugin`](./chain-plugin.md) to the RPC API interface managed by the [`http_plugin`](./http-plugin/index.md).

## Usage

```console
# config.ini
plugin = eosio::chain_api_plugin
```
```sh
# command-line
nodeos ... --plugin eosio::chain_api_plugin
```

## Options

None

## Dependencies

* [`chain_plugin`](./chain-plugin.md)
* [`http_plugin`](./http-plugin.md)

### Load Dependency Examples

```console
# config.ini
plugin = eosio::chain_plugin
[options]
plugin = eosio::http_plugin
[options]
```
```sh
# command-line
nodeos ... --plugin eosio::chain_plugin [operations] [options]  \
           --plugin eosio::http_plugin [options]
```
