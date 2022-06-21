# Terminet - CertiK(Shentu) API User Guide

> A decentralized, member-controlled system for compensating any blockchain network for lost or stolen cryptocurrency. Shentu-Certik includes the mainnet and Yulei testnet. If you don't want to deploy a node and just want to call the node API for use, Terminet provides a non-verified RPC node.



*CertiK Chain* (renamed Shentu Chain) is a Delegated Proof of Stake (DPoS) blockchain infrastructure built using the Cosmos SDK. The *Project* is designed to provide code security for other blockchain underlay and decentralized applications

If you don't want to deploy a CertiK node and just want to call the node API for use, Terminet provides a public and reliable RPC node for developers to use for free.



Terminet has joined the CertiK ecosystem as a network validator, and welcome everyone to delegate CTK to Terminet validators

> Validator Name: Terminet
>
> Validator Address: certikvaloper1m7p5lavl8dkjrzc35q980c99x29wj4xwhwj0wq
>
> https://explorer.shentu.technology/validators/certikvaloper1m7p5lavl8dkjrzc35q980c99x29wj4xwhwj0wq?net=shentu-2.2



## API Configuration

Shentu-Certik mainnet API

https://certikapi.terminet.io



## Simple Data API Example

The following are the reference API parameters:

```shell
https://certikapi.terminet.io/abci_info?
https://certikapi.terminet.io/abci_query?path=_&data=_&height=_&prove=_
https://certikapi.terminet.io/block?height=_
https://certikapi.terminet.io/block_by_hash?hash= _
https://certikapi.terminet.io/block_results?height=_
https://certikapi.terminet.io/block_search?query=_&page=_&per_page=_&order_by=_
https://certikapi.terminet.io/blockchain?minHeight =_&maxHeight=_
https://certikapi.terminet.io/broadcast_evidence?evidence=_
https://certikapi.terminet.io/broadcast_tx_async?tx=_
https://certikapi.terminet.io/broadcast_tx_commit?tx=_
https://certikapi.terminet.ioi/broadcast_tx_sync?tx=_
https://certikapi.terminet.io/check_tx?tx=_
https://certikapi.terminet.io/commit?height=_
https://certikapi.terminet.io/consensus_params?height=_
https://certikapi.terminet.io/consensus_state?
https://certikapi.terminet.io/dump_consensus_state?
https://certikapi.terminet.io/genesis?
https://certikapi.terminet.io/genesis_chunked?chunk=_
https://certikapi.terminet.io/health?
https://certikapi.terminet.io/net_info?
https://certikapi.terminet.io/num_unconfirmed_txs?
https://certikapi.terminet.io/status?
https://certikapi.terminet.io/subscribe?query=_
https://certikapi.terminet.io/tx?hash=_&prove=_
https://certikapi.terminet.io/tx_search?query=_&prove=_&page= _&per_page=_&order_by=_
https://certikapi.terminet.io/unconfirmed_txs?limit=_
https://certikapi.terminet.io/unsubscribe?query=_
https://certikapi.terminet.io/unsubscribe_all?
https://certikapi.terminet.io/validators?height=_&page=_&per_page=_
```

##### 1、View node network status

```shell
# Request
$ curl -s https://certikapi.terminet.io/status
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "node_info": {
      "protocol_version": {
        "p2p": "8",
        "block": "11",
        "app": "0"
      },
      "id": "78a1618510966e8301896fd3217223ad3247a2eb",
      "listen_addr": "tcp://0.0.0.0:26656",
      "network": "shentu-2.2",
      "version": "0.34.14",
      "channels": "40202122233038606100",
      "moniker": "al4-sgp-shentu-002",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://0.0.0.0:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "FB9C9EFEE32D9854535946E1262D2C6EBFA3D9D9D134CBD987DCB88EE56ACD3A",
      "latest_app_hash": "07288183BB7455CB42212DECD2153AD4D12ED004F0D792E101885B3ED7206EE9",
      "latest_block_height": "8421932",
      "latest_block_time": "2022-06-17T02:36:42.490950537Z",
      "earliest_block_hash": "4795D0CAF46F429C318B522E99A73112D918BDE48C86891DB35B1348B481DAE9",
      "earliest_app_hash": "610DC479FBFB6420EAB380B8EC00F0B718EB6E71E34EA55902F5EBD57121711B",
      "earliest_block_height": "8409001",
      "earliest_block_time": "2022-06-16T04:40:33.758261291Z",
      "catching_up": false
    },
    "validator_info": {
      "address": "59C231449D57E31C4F6FBF81CA8DE70966EC71AE",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "7OQohuPbqmqsnKxanvWoJmJEqt8dCbC4iSSxm+uv6tI="
      },
      "voting_power": "0"
    }
  }
}
```

##### 2、View a block information

```shell
# Request
$ curl -s https://certikapi.terminet.io/block_results?height=8421960
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "height": "8421960",
    "txs_results": null,
    "begin_block_events": [
      {
        "type": "coin_received",
        "attributes": [
          {
            "key": "cmVjZWl2ZXI=",
            "value": "Y2VydGlrMW0zaDMwd2x2c2Y4bGxydXh0cHVrZHZzeTBrbTJrdW04MGVtMHh0",
            "index": true
          },
          {
            "key": "YW1vdW50",
            "value": "MTc2MDQ3OHVjdGs=",
            "index": true
          }
        ]
      },
      {
        "type": "coinbase",
        "attributes": [
          {
            "key": "bWludGVy",
            "value": "Y2VydGlrMW0zaDMwd2x2c2Y4bGxydXh0cHVrZHZzeTBrbTJrdW04MGVtMHh0",
            "index": true
          },
          {
            "key": "YW1vdW50",
            "value": "MTc2MDQ3OHVjdGs=",
            "index": true
          }
        ]
      },
      {
            "key": "aGVpZ2h0",
            "value": "ODQyMTk2MA==",
            "index": true
          }
        ]
      }
    ],
    "end_block_events": [
      {
        "type": "aggregate_task",
        "attributes": [
          {
            "key": "Y29udHJhY3Q=",
            "value": "YnNjOjB4ZTdmMTU1OTdiNzU5NGUxNTE2OTUyMDAxZjU3ZDAyMmJhNzk5YjQ3OQ==",
            "index": true
          },
          {
            "key": "c3RhdHVz",
            "value": "VEFTS19TVEFUVVNfU1VDQ0VFREVE",
            "index": true
          }
        ]
      }
    ],
    "validator_updates": null,
    "consensus_param_updates": {
      "block": {
        "max_bytes": "22020096",
        "max_gas": "-1"
      },
      "evidence": {
        "max_age_num_blocks": "100000",
        "max_age_duration": "172800000000000"
      },
      "validator": {
        "pub_key_types": [
          "ed25519"
        ]
      }
    }
  }
}
```

##### 3、View connected node information

```shell
# Request
$ curl -s https://certikapi.terminet.io/net_info
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "listening": true,
    "listeners": [
      "Listener(@)"
    ],
    "n_peers": "8",
    "peers": [
      {
        "node_info": {
          "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
          },
          "id": "606257c520e7460218d478e6cdce2d3e08ecb0bb",
          "listen_addr": "tcp://0.0.0.0:26656",
          "network": "shentu-2.2",
          "version": "0.34.14",
          "channels": "40202122233038606100",
          "moniker": "ip-172-31-22-76",
          "other": {
            "tx_index": "on",
            "rpc_address": "tcp://0.0.0.0:26657"
          }
        },
        "is_outbound": true,
        "connection_status": {
          "Duration": "605221463498",
          "SendMonitor": {
            "Start": "2022-06-17T02:36:27.22Z",
            "Bytes": "3923054",
            "Samples": "1658",
            "InstRate": "1115",
            "CurRate": "19214",
            "AvgRate": "6482",
            "PeakRate": "226690",
            "BytesRem": "0",
            "Duration": "605220000000",
            "Idle": "200000000",
            "TimeRem": "0",
            "Progress": 0,
            "Active": true
          },
          "RecvMonitor": {
            "Start": "2022-06-17T02:36:27.22Z",
            "Bytes": "7789925",
            "Samples": "1580",
            "InstRate": "1450",
            "CurRate": "28165",
            "AvgRate": "12871",
            "PeakRate": "323820",
            "BytesRem": "0",
            "Duration": "605220000000",
            "Idle": "140000000",
            "TimeRem": "0",
            "Progress": 0,
            "Active": true
          },
          "Channels": [
            {
              "ID": 48,
              "SendQueueCapacity": "1",
              "SendQueueSize": "0",
              "Priority": "5",
              "RecentlySent": "368"
            },
            {
              "ID": 64,
              "SendQueueCapacity": "1000",
              "SendQueueSize": "0",
              "Priority": "5",
              "RecentlySent": "0"
            },
            {
              "ID": 0,
              "SendQueueCapacity": "10",
              "SendQueueSize": "0",
              "Priority": "1",
              "RecentlySent": "0"
            }
          ]
        },
        "remote_ip": "65.108.127.50"
      }
    ]
  }
}
```

##### 4、View validator information for a block

```shell
# Request
$ curl -s https://certikapi.terminet.io/validators?height=8422060
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "block_height": "8422060",
    "validators": [
      {
        "address": "7D051D24F7D5C0C98144AA9DD44B79ABC10234D2",
        "pub_key": {
          "type": "tendermint/PubKeyEd25519",
          "value": "lzn2Q4Z4DfUdrIDdxVOcXQS44gEdlLpL8T3QnO4brZk="
        },
        "voting_power": "6326711",
        "proposer_priority": "14650066"
      },
      {
        "address": "BE8A53AF154FB2FB48B0CC99161D73AD1520F2EB",
        "pub_key": {
          "type": "tendermint/PubKeyEd25519",
          "value": "YdGqDGExPZr+EDZ3Onjt+dxv46QlhhWHD64Lci/lKlI="
        },
        "voting_power": "192938",
        "proposer_priority": "-3597235"
      }
    ],
    "count": "30",
    "total": "120"
  }
}
```



---



If you have any questions, please feel free to reach out to us using our social media channels as below

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)

## About Terminet

Terminet is a professional Staking and ecosystem service provider. The core team consists of experienced developers, product and operations experts and blockchain enthusiasts with more than 5 years of experience in the blockchain industry.

Terminet has years of technical expertise in distributed high performance computing, node infrastructure, network, hardware, API support and storage for the Web 3 ecosystem, and is dedicated to helping investors and ordinary Token holders profit from digital assets. Build a reliable, secure and easy-to-use web3 infrastructure service layer to lower web3 connecting barriers and web3 application development thresholds, powering web3 development.