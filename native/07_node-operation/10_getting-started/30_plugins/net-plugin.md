---
title: net_plugin
dont_translate_title: true
---

## Overview

The `net_plugin` offers an authenticated peer-to-peer (p2p) protocol for continuous synchronization of nodes. It also implements the core functionality provided by the [Net API plugin](./net-api-plugin.md).

## Usage

```console
# config.ini
plugin = eosio::net_plugin
[options]
```
```sh
# command-line
nodeos ... --plugin eosio::net_plugin [options]
```

## Options

These can be specified from either the command-line or the `config.ini` file:

### Config Options for `net_plugin`

Option (=default) | Description
-|-
`--p2p-listen-endpoint arg (=0.0.0.0:9876)` | The actual host:port used to listen for incoming p2p connections.
`--p2p-server-address arg` | An externally accessible host:port for identifying this node. Defaults to p2p-listen-endpoint.
`--p2p-peer-address arg` | The public endpoint of a peer node to connect to. Use multiple p2p-peer-address options as needed to compose a network. Syntax: host:port[:^trx^\|^blk^] The optional 'trx' and 'blk' indicates to node that only transactions 'trx' or blocks 'blk' should be sent.  Examples: p2p.vaulta.io:9876 p2p.trx.vaulta.io:9876:trx p2p.blk.vaulta.io:9876:blk
`--p2p-max-nodes-per-host arg (=1)` | Maximum number of client nodes from any single IP address
`--p2p-accept-transactions arg (=1)` | Allow transactions received over p2p network to be evaluated and relayed if valid.
`--p2p-auto-bp-peer arg` | The account and public p2p endpoint of a block producer node to automatically connect to when the it is in producer schedule proximity .   Syntax: account,host:port Example, vaultaprod1,p2p.vaulta.io:9876 vaultaprod2,p2p.trx.vaulta.io:9876:t rx vaultaprod3,p2p.blk.vaulta.io:9876:b lk
`--agent-name arg (=Vaulta Test Agent)` | The name supplied to identify this node amongst the peers.
`--allowed-connection arg (=any)` | Can be 'any' or 'producers' or 'specified' or 'none'. If 'specified', peer-key must be specified at least once. If only 'producers', peer-key is not required. 'producers' and 'specified' may be combined.
`--peer-key arg` | Optional public key of peer allowed to connect.  May be used multiple times.
`--peer-private-key arg` | Tuple of [PublicKey, WIF private key] (may specify multiple times)
`--max-clients arg (=25)` | Maximum number of clients from which connections are accepted, use 0 for no limit
`--connection-cleanup-period arg (=30)` | number of seconds to wait before cleaning up dead connections
`--max-cleanup-time-msec arg (=10)` | max connection cleanup time per cleanup call in milliseconds
`--p2p-dedup-cache-expire-time-sec arg (=10)` | Maximum time to track transaction for duplicate optimization
`--net-threads arg (=4)` | Number of worker threads in net_plugin thread pool
`--sync-fetch-span arg (=100)` | number of blocks to retrieve in a chunk from any individual peer during synchronization
`--use-socket-read-watermark arg (=0)` | Enable experimental socket read watermark optimization
`--peer-log-format arg (=["${_name}" - ${_cid} ${_ip}:${_port}] )` | The string used to format peers when logging messages about them.  Variables are escaped with ${^variable name^}. Available Variables: _name  self-reported name _cid   assigned connection id _id    self-reported ID (64 hex characters) _sid   first 8 characters of _peer.id _ip    remote IP address of peer _port  remote port number of peer _lip   local IP address connected to peer _lport local port number connected to peer
`--p2p-keepalive-interval-ms arg (=10000)` | peer heartbeat keepalive message interval in milliseconds

## Dependencies

None
