# Terminet-IRISnet节点API使用说明

IRISnet旨在提供便于构建分布式商业应用程序的技术基础，是更大的Cosmos网络的一部分。网络中所有分区都能够通过标准IBC协议与Cosmos网络中的任何其他分区进行交互，是一个基于Cosmos SDK和Tendermint构建的PoS区块链。

如果你不想部署节点，只想调用节点API以供使用，Terminet提供了一个公开可靠的RPC节点供社区使用。



*您可以参考本文档提供的API地址连接到IRIS网络，并通过相关调用方法查询链上数据。*



**欢迎委托IRIS 给Terminet验证人**

> 验证人名称: Terminet
>
> 验证人地址：iva194s9c46x6vr9drynsm5dupkeleqzejtszmxdnx
>
> https://irishub.iobscan.io/#/staking/iva194s9c46x6vr9drynsm5dupkeleqzejtszmxdnx



## API配置

##### IRISnet主网API

https://irisapi.terminet.io



## 简单数据API示例

以下是可参考API参数：

```shell
https://irisapi.terminet.io/abci_info?
https://irisapi.terminet.io/abci_query?path=_&data=_&height=_&prove=_
https://irisapi.terminet.io/block?height=_
https://irisapi.terminet.io/block_by_hash?hash= _
https://irisapi.terminet.io/block_results?height=_
https://irisapi.terminet.io/block_search?query=_&page=_&per_page=_&order_by=_
https://irisapi.terminet.io/blockchain?minHeight =_&maxHeight=_
https://irisapi.terminet.io/broadcast_evidence?evidence=_
https://irisapi.terminet.io/broadcast_tx_async?tx=_
https://irisapi.terminet.io/broadcast_tx_commit?tx=_
https://irisapi.terminet.ioi/broadcast_tx_sync?tx=_
https://irisapi.terminet.io/check_tx?tx=_
https://irisapi.terminet.io/commit?height=_
https://irisapi.terminet.io/consensus_params?height=_
https://irisapi.terminet.io/consensus_state?
https://irisapi.terminet.io/dump_consensus_state?
https://irisapi.terminet.io/genesis?
https://irisapi.terminet.io/genesis_chunked?chunk=_
https://irisapi.terminet.io/health?
https://irisapi.terminet.io/net_info?
https://irisapi.terminet.io/num_unconfirmed_txs?
https://irisapi.terminet.io/status?
https://irisapi.terminet.io/subscribe?query=_
https://irisapi.terminet.io/tx?hash=_&prove=_
https://irisapi.terminet.io/tx_search?query=_&prove=_&page= _&per_page=_&order_by=_
https://irisapi.terminet.io/unconfirmed_txs?limit=_
https://irisapi.terminet.io/unsubscribe?query=_
https://irisapi.terminet.io/unsubscribe_all?
https://irisapi.terminet.io/validators?height=_&page=_&per_page=_
```

##### 1、查看节点网络状态

```shell
# Request
$ curl -s https://irisapi.terminet.io/status
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
      "id": "9cd1fe0109d347d49976a7353f8adcd308211f08",
      "listen_addr": "tcp://0.0.0.0:26656",
      "network": "irishub-1",
      "version": "0.34.12",
      "channels": "40202122233038606100",
      "moniker": "CHANGE THIS TO YOUR MONIKER",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://0.0.0.0:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "8BE4984907967D415925BA4186EB8090B451080F27311218006205DB4FED7E14",
      "latest_app_hash": "4AE9940101B4D68B52775A2D20B24570CE0B0CADA079409233950A035DC45B1D",
      "latest_block_height": "15375067",
      "latest_block_time": "2022-06-17T03:01:42.336770203Z",
      "earliest_block_hash": "8ED4EC565620B7563041DB81C2EEAE285217FEC3BAC707A9C624AC203A20DC27",
      "earliest_app_hash": "F39B1BC0DD435800058FCCDC0FB044CBA25E2AC4AE2153AEF1B1DEF2BBF26D02",
      "earliest_block_height": "15367001",
      "earliest_block_time": "2022-06-16T12:32:06.188580663Z",
      "catching_up": false
    },
    "validator_info": {
      "address": "F9CA5B1FB57B1D6824225F0213C5CB7CC42DEF94",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "Vb7ndFiDFVh+q2eSlXb73gR/2A8ANnCGoDVbGPJlweg="
      },
      "voting_power": "0"
    }
  }
}
```

##### 2、查看某个块信息

```shell
# Request
$ curl -s https://irisapi.terminet.io/block_results?height=15375176
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "height": "15375176",
    "txs_results": null,
    "begin_block_events": [
      {
        "type": "coin_received",
        "attributes": [
          {
            "key": "cmVjZWl2ZXI=",
            "value": "aWFhMW0zaDMwd2x2c2Y4bGxydXh0cHVrZHZzeTBrbTJrdW04YW44Zjkz",
            "index": true
          },
          {
            "key": "YW1vdW50",
            "value": "MTI2NzUyMzV1aXJpcw==",
            "index": true
          }
        ]
      },
      {
        "type": "coinbase",
        "attributes": [
          {
            "key": "bWludGVy",
            "value": "aWFhMW0zaDMwd2x2c2Y4bGxydXh0cHVrZHZzeTBrbTJrdW04YW44Zjkz",
            "index": true
          },
          {
            "key": "YW1vdW50",
            "value": "MTI2NzUyMzV1aXJpcw==",
            "index": true
          }
        ]
      },
      {
        "type": "liveness",
        "attributes": [
          {
            "key": "YWRkcmVzcw==",
            "value": "aWNhMXBocmg1dHU5NzRyNjQyaGZxbjdneDluNWY4ODlzOTNsc2V6dTlu",
            "index": true
          },
          {
            "key": "bWlzc2VkX2Jsb2Nrcw==",
            "value": "ODY4NA==",
            "index": true
          },
          {
            "key": "aGVpZ2h0",
            "value": "MTUzNzUxNzY=",
            "index": true
          }
        ]
      }
    ],
    "end_block_events": null,
    "validator_updates": null,
    "consensus_param_updates": {
      "block": {
        "max_bytes": "2000000",
        "max_gas": "-1"
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
$ curl -s https://irisapi.terminet.io/net_info
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "listening": true,
    "listeners": [
      "Listener(@)"
    ],
    "n_peers": "11",
    "peers": [
      {
        "node_info": {
          "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
          },
          "id": "3dcd46611a6a3d0aa22a4738a3e70dcc5c2a4d34",
          "listen_addr": "tcp://0.0.0.0:26656",
          "network": "irishub-1",
          "version": "0.34.12",
          "channels": "40202122233038606100",
          "moniker": "Conan",
          "other": {
            "tx_index": "on",
            "rpc_address": "tcp://0.0.0.0:26657"
          }
        },
        "is_outbound": true,
        "connection_status": {
          "Duration": "865616530455",
          "SendMonitor": {
            "Start": "2022-06-17T03:01:36.7Z",
            "Bytes": "7814193",
            "Samples": "2528",
            "InstRate": "8219",
            "CurRate": "7907",
            "AvgRate": "9027",
            "PeakRate": "360520",
            "BytesRem": "0",
            "Duration": "865620000000",
            "Idle": "2680000000",
            "TimeRem": "0",
            "Progress": 0,
            "Active": true
          },
          "RecvMonitor": {
            "Start": "2022-06-17T03:01:36.7Z",
            "Bytes": "9313121",
            "Samples": "1759",
            "InstRate": "2713",
            "CurRate": "11341",
            "AvgRate": "10568",
            "PeakRate": "194500",
            "BytesRem": "0",
            "Duration": "881220000000",
            "Idle": "1020000000",
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
              "RecentlySent": "272"
            },
            {
              "ID": 97,
              "SendQueueCapacity": "10",
              "SendQueueSize": "0",
              "Priority": "3",
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
        "remote_ip": "35.221.235.168"
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