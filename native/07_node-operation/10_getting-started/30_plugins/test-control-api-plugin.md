---
title: test_control_api_plugin
dont_translate_title: true
---

See [Test Control API Reference](https://docs.vaulta.com/apis/leap/latest/test_control.api/)

## Overview

The `test_control_api_plugin` allows to forward control messages to the [test_control_plugin](./test-control-plugin.md). The current endpoint instructs the plugin to initiate the graceful shutdown of the `nodeos` instance once a specific block is reached. This functionality is primarily designed for testing purposes.

## Usage

```console
# config.ini
plugin = eosio::test_control_api_plugin
```
```sh
# command-line
nodeos ... --plugin eosio::test_control_api_plugin
```

## Options

None

## Usage Example

```sh
curl %s/v1/test_control/kill_node_on_producer -d '{ \"producer\":\"%s\", \"where_in_sequence\":%d, \"based_on_lib\":\"%s\" }' -X POST -H \"Content-Type: application/json\"" %
```

## Dependencies

* [`test_control_plugin`](./test-control-plugin.md)
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
