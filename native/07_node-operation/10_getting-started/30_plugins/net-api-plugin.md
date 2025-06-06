---
title: net_api_plugin
dont_translate_title: true
---

See [Net API Reference](https://docs.vaulta.com/apis/leap/latest/net.api/).

## Overview

The `net_api_plugin` exposes functionality from the [`net_plugin`](./net-plugin.md) to the RPC API interface managed by the [`http_plugin`](./http-plugin.md). The `net_api_plugin` allows node operators to manage the peer-to-peer (p2p) connections of an active node.

The `net_api_plugin` provides four RPC API endpoints:
* connect
* disconnect
* connections
* status

> ⚠️ This plugin exposes endpoints that allow management of p2p connections. Running this plugin on a publicly accessible node is not recommended as it can be exploited.

## Usage

```console
# config.ini
plugin = eosio::net_api_plugin
```
```sh
# command-line
nodeos ... --plugin eosio::net_api_plugin
```

## Options

None

## Dependencies

* [`net_plugin`](./net-plugin.md)
* [`http_plugin`](./http-plugin.md)

### Load Dependency Examples

```console
# config.ini
plugin = eosio::net_plugin
[options]
plugin = eosio::http_plugin
[options]
```
```sh
# command-line
nodeos ... --plugin eosio::net_plugin [options]  \
           --plugin eosio::http_plugin [options]
```
