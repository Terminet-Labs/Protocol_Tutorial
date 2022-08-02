# Terminet-Gravity Bridge节点API使用说明

Gravity Bridge是一个开放的、去中心化的跨链桥，提供释放区块链生态系统之间的互操作性和流动性的力量。Gravity Bridge链充当以太坊到Cosmos的桥梁，许多不同的基于Cosmos的项目可以连接并使用bridge访问ERC20资产。目前已经有 5 个 Cosmos 区块链网络，如 Osmosis、Stargaze 等宣布接入Gravity Bridge。

我们相信Gravity Bridge 将成为Cosmos与以太坊两大生态间最重要的价值流动桥梁，具备很大的发展潜力。



***如果你不想部署节点，只是想简单的调用节点API，Terminet提供了一个非验证的RPC节点。您可以参考本文档提供的API连接到Gravity Bridge协议，并通过相关调用方法查询链上数据。***



**欢迎委托 GRAVITON 给Terminet验证人**

> 验证人名称: Terminet
>
> 验证人地址：gravityvaloper1qx0hhh50pj5cxx99ql2vsczslsx64wdmrqqs7h
>
> https://www.mintscan.io/gravity-bridge/validators/gravityvaloper1qx0hhh50pj5cxx99ql2vsczslsx64wdmrqqs7h



## API

Gravity Bridge 主网API：https://gravityapi.terminet.io

##### 如下是可调用参数：

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



## 简单数据API示例

##### 1、查看节点网络状态

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

##### 2、查看某个块信息

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

##### 3、查看连接的节点信息

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



如果您有任何问题，请随时使用我们的社交媒体渠道与我们联系，联系方式如下：

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)

## 关于Terminet

Terminet 是一家专业的 Staking 和生态系统服务提供商。核心团队由经验丰富的开发人员、产品和运营专家以及区块链行业5年以上经验的区块链爱好者组成。
Terminet在分布式高性能计算、节点基础设施、网络、硬件、API支持和Web 3生态系统的存储等方面有多年技术积累，致力于帮助投资者和普通 Token 持有者从数字资产中获利。构建可靠、安全、易用的 web3 基础设施服务层，降低 web3 连接壁垒和 web3 应用开发门槛，为 web3 开发赋能。

