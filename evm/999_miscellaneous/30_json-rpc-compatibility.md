---
title: JSON RPC Compatibility
---

All the JSON-RPC calls are inherently supported thanks to the full functioning Vaulta EVM node which is built based on Silkworm node. However, some methods are blocked in the current phase for the following reasons:

* Some methods are deprecated or discontinued.
* Some methods are designed for the local node scenario. They are not exposed to the public API, however you can access them when you deploy your own Vaulta EVM node.
* Some methods involve complex logic, therefore more tests need to be performed before they will be exposed.

## RPC List

Notes:
* The JSON-RPC calls listed below do NOT include methods that are blocked in the current phase.
* "Vaulta EVM Node-SlowQuery" is designated for nodes dedicated to handle slow or heavy queries. This is done so that those slow queries do not stop or degrade the performance of regular nodes serving other method requests.

| RPC Method                                  | Destination  |
| ------------------------------------------- | ------------ |
| net\_version                                | Vaulta EVM node |
| eth\_blockNumber                            | Vaulta EVM node |
| eth\_chainId                                | Vaulta EVM node |
| eth\_protocolVersion                        | Vaulta EVM node |
| eth\_gasPrice                               | Tx Wrapper   |
| eth\_getBlockByHash                         | Vaulta EVM node |
| eth\_getBlockByNumber                       | Vaulta EVM node |
| eth\_getBlockTransactionCountByHash         | Vaulta EVM node |
| eth\_getBlockTransactionCountByNumber       | Vaulta EVM node |
| eth\_getUncleByBlockHashAndIndex            | Vaulta EVM node |
| eth\_getUncleByBlockNumberAndIndex          | Vaulta EVM node |
| eth\_getUncleCountByBlockHash               | Vaulta EVM node |
| eth\_getUncleCountByBlockNumber             | Vaulta EVM node |
| eth\_getTransactionByHash                   | Vaulta EVM node |
| eth\_getRawTransactionByHash                | Vaulta EVM node |
| eth\_getTransactionByBlockHashAndIndex      | Vaulta EVM node |
| eth\_getRawTransactionByBlockHashAndIndex   | Vaulta EVM node |
| eth\_getTransactionByBlockNumberAndIndex    | Vaulta EVM node |
| eth\_getRawTransactionByBlockNumberAndIndex | Vaulta EVM node |
| eth\_getTransactionReceipt                  | Vaulta EVM node |
| eth\_getBlockReceipts                       | Vaulta EVM node |
| eth\_estimateGas                            | Vaulta EVM node |
| eth\_getBalance                             | Vaulta EVM node |
| eth\_getCode                                | Vaulta EVM node |
| eth\_getTransactionCount                    | Vaulta EVM node |
| eth\_getStorageAt                           | Vaulta EVM node |
| eth\_call                                   | Vaulta EVM node |
| eth\_callBundle                             | Vaulta EVM node |
| eth\_createAccessList                       | Vaulta EVM node |
| eth\_getLogs                                | Vaulta EVM Node-SlowQuery |
| eth\_sendRawTransaction                     | Tx Wrapper   |
| debug\_traceBlockByHash                     | Vaulta EVM Node-SlowQuery |
| debug\_traceBlockByNumber                   | Vaulta EVM Node-SlowQuery |
| debug\_traceTransaction                     | Vaulta EVM Node-SlowQuery |
| debug\_traceCall                            | Vaulta EVM Node-SlowQuery |
| trace\_call                                 | Vaulta EVM Node-SlowQuery |
| trace\_callMany                             | Vaulta EVM Node-SlowQuery |
| trace\_rawTransaction                       | Vaulta EVM Node-SlowQuery |
| trace\_replayBlockTransactions              | Vaulta EVM Node-SlowQuery |
| trace\_replayTransaction                    | Vaulta EVM Node-SlowQuery |
| trace\_block                                | Vaulta EVM Node-SlowQuery |
| trace\_filter                               | Vaulta EVM Node-SlowQuery |
| trace\_get                                  | Vaulta EVM Node-SlowQuery |
| trace\_transaction                          | Vaulta EVM Node-SlowQuery |

## Batched Requests

Sending an array of request objects as the body to the JSON-RPC API is not currently supported. The server will return a 400 error in this case. If this is impacting you, try a workaround until this is supported.

Example failing request body:
```json
[{"method":"eth_chainId","params":[],"id":1,"jsonrpc":"2.0"},{"method":"eth_blockNumber","params":[],"id":2,"jsonrpc":"2.0"}]
```
