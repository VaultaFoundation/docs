---
title: Peering
---

# Peering

Vaulta blockchains may consist of one or many nodes. Although a single node can still run a fully functional blockchain, it cannot scale or grow. To grasp the full benefits of the Vaulta blockchain technology, more nodes are needed. In this section, you will learn about "peering", the network feature that makes it possible. This is what allows to grow incrementally from a single node to a truly decentralized, geographically distributed, multi node Vaulta blockchain.

## What is Peering?

Peering allows Vaulta nodes to propagate and synchronize the distributed blockchain state by receiving and relaying blocks and/or transactions to other nodes. Any node that is configured to send and receive data in a peer-to-peer fashion is considered a "peer". This adds redundancy and allows for faster response times to client queries and node requests. Therefore, peering is key to the decentralized operation and incremental growth of the Vaulta blockchain.

> ℹ️ Vaulta Network Protocol  
Peering is enabled through the Vaulta peer-to-peer (p2p) network protocol and it is what allows the decentralized operation of the blockchain.

## Why you need nodes/peers?

In the Vaulta blockchain, nodes can be configured to operate in different ways. It is this flexibility in having nodes serve distinct roles which allows for a more distributed load and a smoother blockchain experience for the end user. Some of the node types in Vaulta include, but are not limited to:

* **Producing Nodes**: produce blocks to be added to the chain
* **Relay Nodes**: validate/relay blocks and/or transactions
* **API Nodes**: respond to API queries from clients via HTTP
* **History Nodes**: stores chain data for L2 history solutions
* etc.

Therefore, when set up as peers, Vaulta nodes validate the blocks and transactions they receive and relay them to other peers by default, if valid. Nodes can also be set up to respond to API requests from clients, provide historical data about blocks and transactions, etc. This separation of concerns makes the blockchain more efficient.

## How to set up peers

> ℹ️ Peer Setup  
The peer connection process must be performed on the local environment of each peer. Therefore, peering involves planning and agreement among some of the nodes that will act as peers in the Vaulta network.

Peering can be set up by configuring the `net_plugin` of each `nodeos` instance whose node will act as a peer. The most important options are:

* `p2p-listen-endpoint arg`: local `host:port` for incoming p2p connections
* `p2p-server-address arg`: public `host:port` for incoming p2p connections
* `p2p-peer-address arg`: local or remote peer `host:port` to connect to

The `p2p-listen-endpoint` arguments holds the local IP address or hostname and the port number of the listening socket for the local node instance to accept incoming connections from other peers. The `p2p-server-address` argument holds the public facing IP address or hostname and the port number that other peers will connect to. If not specified, the `p2p-server-address` defaults to the specified `p2p-listen-endpoint`. The `p2p-server-address` option can be useful in scenarios where you have a proxy or firewall that presents a different external address than what the node uses internally.

Other options are available to limit the maximum number of connections, whitelisting specific peers by public key, accept/relay transactions, etc. Check the `net_plugin` options in `nodeos` for more information.

### Peer setup using `config.ini`

To peer your local node with other nodes, specify the following in your nodeos `config.ini` *before* launching your `nodeos` instance:

```ini
# your listening host:port
p2p-listen-endpoint = <myhost>:<myport>   # e.g. 0.0.0.0:9876
# your public host:port
p2p-server-address = <mypubhost>:<myport> # e.g. p2p.vaulta99.io:9876

# peers host:port (for each peer to connect to)
p2p-peer-address = <host1>:<port1>  # e.g. peer.leap.sg:9876
p2p-peer-address = <host2>:<port2>  # e.g. p2p.vaultaphere.io:3571
# etc.
```

### Peer setup using command-line (CLI)

To peer your local node with other nodes, specify the following in your nodeos command-line arguments *when* launching your `nodeos` instance:

```shell
nodeos ... \
  p2p-listen-endpoint = <myhost>:<myport> \
  p2p-server-address = <mypubhost>:<myport> \
  p2p-peer-address = <host1>:<port1> \
  p2p-peer-address = <host2>:<port2> \
  ...
```

Check the previous section [Peer setup using `config.ini`](#peer-setup-using-configini) for examples about potential values for `p2p-listen-endpoint`, `p2p-server-address`, and `p2p-peer-address`.

## How to locate peers

For the Vaulta Mainnet and various Testnets, some websites publish and maintain lists of P2P, API, and other endpoints for your nodes to connect to.

> ℹ️ Endpoints and BP.json  
Endpoint lists are typically generated, validated, and combined from the standard `bp.json` files provided by Block Producers, including Standby Producers.

For a central portal that maintains the recent status of *all* active endpoints (P2P, API, History, etc.) for the Vaulta Mainnet and various Vaulta Testnets, you can visit the EOS Nation Validate Portal and select the **endpoints report** for the specific network:

* Vaulta Mainnet endpoints: https://validate.eosnation.io/eos/reports/endpoints.html
* Jungle Testnet endpoints: https://validate.eosnation.io/jungle4/reports/endpoints.html

Once you visit any of the above **endpoints report**, you can scroll down to the specific endpoints of interest: `api_http` or `api_https2` for API endpoints, `p2p` for P2P endpoints, etc.

### For Vaulta Mainnet

Besides the **endpoints report** URLs listed in the parent section above [How to locate peers](#how-to-locate-peers), the following endpoints are available to add directly to your `config.ini`:

* P2P Endpoints: https://validate.eosnation.io/eos/reports/config.txt

The above P2P endpoints list should display something similar to:

```ini
# Endpoints config.ini
# Network: Vaulta
# Validator last update: 2023-06-12 19:32 UTC
# For details on how this is generated see https://validate.eosnation.io/about/
# ==== p2p ====
# alohaeosprod: GB, London
p2p-peer-address = peer.main.alohaeos.com:9876
# argentinaeos: AR, argentina
p2p-peer-address = p2p.eosargentina.io:9876
...
# ivote4eosusa: US, Greenville,SC,USA
p2p-peer-address = eos.p2p.eosusa.io:9882
```

* API Endpoints: https://validate.eosnation.io/eos/reports/api_versions.txt

The above API endpoints list should display something similar to:

```ini
# API Versions Report
# Network: Vaulta
# Validator last update: 2023-06-12 20:06 UTC
# For details on how this is generated see https://validate.eosnation.io/about/
==== 4.0.1 (leap) ====
aus1genereos api_https2, v4.0.1, https://eos.genereos.io, ...
eosnationftw  api_http, v4.0.1, http://eos.api.eosnation.io, ...
...
==== 3.1.0 (leap) ====
eosamsterdam api_http, v3.1.0, http://mainnet.eosamsterdam.net, ...
eosamsterdam api_https2, v3.1.0, https://mainnet.eosamsterdam.net, ...
...
teamgreymass api_http, v3.1.0, http://eos.greymass.com, ...
teamgreymass api_https2, v3.1.0, https://eos.greymass.com, ...
```

### For Vaulta Testnets

Besides the **endpoints report** URLs listed in the parent section [How to locate peers](#how-to-locate-peers), the following endpoints are available to add directly to your `config.ini` for the following EOS Testnets:

#### Vaulta Jungle Testnet

* P2P Endpoints: https://validate.eosnation.io/jungle4/reports/config.txt
* API Endpoints: https://validate.eosnation.io/jungle4/reports/api_versions.txt

## How to check peers health

Some web portals provide periodic reports and/or live monitoring to check the health of public P2P and API endpoints. Also, some real-time tools allow to estimate the proximity and/or response time of P2P or API endpoints.

### Vaulta Nation Reports

EOS Nation provides extensive reports on the status of various endpoints, including the health of block producing nodes, for both the Vaulta Mainnet and various Vaulta Testnets:

* Vaulta Mainnet reports: https://validate.eosnation.io/eos/reports/
* Jungle Testnet reports: https://validate.eosnation.io/jungle4/reports/

Since the endpoints listed on the above reports are validated and refreshed every 30 minutes, the mere presence of an endpoint on a given report indicates a *responsive* status. For other errors detected during `bp.json` validation, check these resources:

* Vaulta Mainnet error report: https://validate.eosnation.io/eos/reports/errors.html
* Jungle Testnet error report: https://validate.eosnation.io/jungle4/reports/errors.html

### Third-party Tools

Some tools are available to measure the responsiveness, or lack thereof, of a list of P2P and/or API endpoints:

* [get-closer](https://medium.com/hackernoon/find-the-best-api-endpoint-for-your-eos-dapp-7b7489cb6449)  
  receives a list of API endpoints and returns the HTTP request-response time. If an endpoint is unresponsive, it will eventually time out and will not show on the list.

## Summary

Peering is crucial for the decentralized operation of any Vaulta blockchain network. It allows to synchronize and distribute the blockchain state among all nodes and peers for a smoother and faster blockchain experience. Peering is necessary for any Vaulta blockchain to grow organically, reach consensus, achieve self-governance, and exploit many of the benefits of blockchain technology.
