# Terminet-Gravity Bridge Node API User Guide

Gravity Bridge is an open, decentralized cross-chain bridge that provides the power to unlock interoperability and liquidity between blockchain ecosystems.

The Gravity Bridge chain acts as a bridge from Ethereum to Cosmos, and many different Cosmos-based projects can connect and use the bridge to access ERC20 assets. There are already 5 Cosmos blockchain networks, such as Osmosis, Stargaze, and others, that have announced access to Gravity Bridge.

We believe Gravity Bridge will be the most important bridge for value flow between Cosmos and Ethereum ecosystems and has great potential for growth.



***If you don't want to deploy a node and just want to simply call the node API, Terminet provides a non-validated RPC node. You can refer to the API provided in this document to connect to the Gravity Bridge protocol and query the data on the chain through the relevant call methods.***



**Welcome to delegate GRAVITON to Terminet validator**.

> Validator name: Terminet
>
> Validator Address: gravityvaloper1qx0hhh50pj5cxx99ql2vsczslsx64wdmrqqs7h
>
> https://www.mintscan.io/gravity-bridge/validators/gravityvaloper1qx0hhh50pj5cxx99ql2vsczslsx64wdmrqqs7h



## API

Gravity Bridge Mainnet API: https://gravityapi.terminet.io

##### The following are callable parameters:

```shell
https://gravityapi.terminet.io/abci_info?
https://gravityapi.terminet.io/abci_query?path=_&data=_&height=_&prove=_
https://gravityapi.terminet.io/block?height=_
https://gravityapi.terminet.io/block_by_hash?hash= _
https://gravityapi.terminet.io/block_results?height=_
https://gravityapi.terminet.io/block_search?query=_&page=_&per_page=_&order_by=_
https://gravityapi.terminet.io/blockchain?minHeight =_&maxHeight=_
https://gravityapi.terminet.io/broadcast_evidence?evidence=_
https://gravityapi.terminet.io/broadcast_tx_async?tx=_
https://gravityapi.terminet.io/broadcast_tx_commit?tx=_
https://gravityapi.terminet.ioi/broadcast_tx_sync?tx=_
https://gravityapi.terminet.io/check_tx?tx=_
https://gravityapi.terminet.io/commit?height=_
https://gravityapi.terminet.io/consensus_params?height=_
https://gravityapi.terminet.io/consensus_state?
https://gravityapi.terminet.io/dump_consensus_state?
https://gravityapi.terminet.io/genesis?
https://gravityapi.terminet.io/genesis_chunked?chunk=_
https://gravityapi.terminet.io/health?
https://gravityapi.terminet.io/net_info?
https://gravityapi.terminet.io/num_unconfirmed_txs?
https://gravityapi.terminet.io/status?
https://gravityapi.terminet.io/subscribe?query=_
https://gravityapi.terminet.io/tx?hash=_&prove=_
https://gravityapi.terminet.io/tx_search?query=_&prove=_&page= _&per_page=_&order_by=_
https://gravityapi.terminet.io/unconfirmed_txs?limit=_
https://gravityapi.terminet.io/unsubscribe?query=_
https://gravityapi.terminet.io/unsubscribe_all?
https://gravityapi.terminet.io/validators?height=_&page=_&per_page=_
```



## Simple Data API Example

##### 1. View the node network status

```shell
# Request
$curl -s https://gravityapi.terminet.io/status
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
      "id": "b165be36e7b96d7eeaf2c6061deccd987f74bc7f",
      "listen_addr": "tcp://0.0.0.0:26656",
      "network": "gravity-bridge-3",
      "version": "0.34.14",
      "channels": "40202122233038606100",
      "moniker": "Terminetapi",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://0.0.0.0:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "67DA42D2CE0EB524FCA16A1D20DA490110A63D1F0EE06A5D4C24E4DE5B741139",
      "latest_app_hash": "418E678484762A4A21DFCDC42E6069058F89FB2A2BC9B0B8ED3756348282645D",
      "latest_block_height": "1459561",
      "latest_block_time": "2022-03-31T21:52:51.274172626Z",
      "earliest_block_hash": "74B590C83F260AEE720A4E303490A1D5E8CC28DCED149B44482A4AADB790B641",
      "earliest_app_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "earliest_block_height": "487540",
      "earliest_block_time": "2021-12-10T14:45:49.658958945Z",
      "catching_up": true
    },
    "validator_info": {
      "address": "E18447D18802F7D0BD81DDBB9439C94D4C3596F9",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "JZj36ZCFQ/EkcBgZHWWtYMWI+3akqSeeTcvvdaW2JHI="
      },
      "voting_power": "0"
    }
  }
}
```

##### 2. View a block information

```shell
# Request
$ curl -s https://gravityapi.terminet.io/block_results?height=1457800
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "height": "1457800",
    "txs_results": null,
    "begin_block_events": [
      {
        "type": "coin_received",
        "attributes": [
          {
            "key": "cmVjZWl2ZXI=",
            "value": "Z3Jhdml0eTFtM2gzMHdsdnNmOGxscnV4dHB1a2R2c3kwa20ya3VtOHZwNHF6Zw==",
            "index": true
          },
          {
            "key": "YW1vdW50",
            "value": "NTMxNzMzODl1Z3Jhdml0b24=",
            "index": true
          }
        ]
      },
      {
        "type": "coinbase",
        "attributes": [
          {
            "key": "bWludGVy",
            "value": "Z3Jhdml0eTFtM2gzMHdsdnNmOGxscnV4dHB1a2R2c3kwa20ya3VtOHZwNHF6Zw==",
            "index": true
          },
          {
            "key": "aGVpZ2h0",
            "value": "MTQ1NzgwMA==",
            "index": true
          }
        ]
      }
    ],
    "end_block_events": null,
    "validator_updates": null,
    "consensus_param_updates": {
      "block": {
        "max_bytes": "100000000",
        "max_gas": "2000000000"
      },
      "evidence": {
        "max_age_num_blocks": "100000",
        "max_age_duration": "172800000000000",
        "max_bytes": "1048576"
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

##### 3. View the connected node information

```shell
# Request
$ curl -s https://gravityapi.terminet.io/net_info
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "listening": true,
    "listeners": [
      "Listener(@)"
    ],
    "n_peers": "10",
    "peers": [
      {
        "node_info": {
          "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
          },
          "id": "5f81c16ebeb7a74f81ca14d458f23b1595732034",
          "listen_addr": "tcp://0.0.0.0:26656",
          "network": "gravity-bridge-3",
          "version": "0.34.14",
          "channels": "40202122233038606100",
          "moniker": "RPC_Grav",
          "other": {
            "tx_index": "on",
            "rpc_address": "tcp://0.0.0.0:26657"
          }
        },
        "is_outbound": true,
        "connection_status": {
          "Duration": "64623698259926",
          "SendMonitor": {
            "Start": "2022-07-03T08:36:21.74Z",
            "Bytes": "82893",
            "Samples": "7104",
            "InstRate": "0",
            "CurRate": "0",
            "AvgRate": "1",
            "PeakRate": "13932",
            "BytesRem": "0",
            "Duration": "64623700000000",
            "Idle": "3480000000",
            "TimeRem": "0",
            "Progress": 0,
            "Active": true
          },
          "RecvMonitor": {
            "Start": "2022-07-03T08:36:21.74Z",
            "Bytes": "65895591",
            "Samples": "162724",
            "InstRate": "524",
            "CurRate": "800",
            "AvgRate": "1020",
            "PeakRate": "29920",
            "BytesRem": "0",
            "Duration": "64623660000000",
            "Idle": "40000000",
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
        "remote_ip": "212.68.160.178"
      }
    ]
  }
}
```



---



If you have any questions, please feel free to reach out to us using our social media channels as below

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)

## About Terminet

Terminet is a professional Staking and ecosystem service provider. The core team consists of experienced developers, product and operations experts and blockchain enthusiasts with more than 5 years of experience in the blockchain industry.

Terminet has years of technical expertise in distributed high performance computing, node infrastructure, network, hardware, API support and storage for the Web 3 ecosystem, and is dedicated to helping investors and ordinary Token holders profit from digital assets. Build a reliable, secure and easy-to-use web3 infrastructure service layer to lower web3 connecting barriers and web3 application development thresholds, powering web3 development.