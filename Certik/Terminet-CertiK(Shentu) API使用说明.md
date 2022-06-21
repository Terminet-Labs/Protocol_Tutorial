# Terminet - CertiK(Shentu) API使用说明



*CertiK Chain*(已更名为Shentu Chain)是一个使用Cosmos SDK构建的委托权益证明（DPoS）区块链基础设施。*项目*旨在为其他区块链底层和去中心化应用程序提供代码安全保障

如果你不想部署CertiK节点，只想调用节点API以供使用，Terminet提供了一个公开可靠的RPC节点供社区免费使用。



Terminet已经加入CertiK生态成为网络验证节点，欢迎大家委托CTK给给Terminet验证人

> 验证人名称: Terminet
>
> 验证人地址：certikvaloper1m7p5lavl8dkjrzc35q980c99x29wj4xwhwj0wq
>
> https://explorer.shentu.technology/validators/certikvaloper1m7p5lavl8dkjrzc35q980c99x29wj4xwhwj0wq?net=shentu-2.2





## API配置

**Certik(Shentu)主网API:**

https://certikapi.terminet.io



## 简单数据API示例

以下是可参考API参数：

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

##### 1、查看节点网络状态

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

##### 2、查看某个块信息

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

##### 3、查看连接的节点信息

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

##### 4、查看某个块的验证者信息

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



如果您有任何问题，请随时使用我们的社交媒体渠道与我们联系，联系方式如下：

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)



## 关于Terminet

Terminet 是一家专业的 Staking 和生态系统服务提供商。核心团队由经验丰富的开发人员、产品和运营专家以及区块链行业5年以上经验的区块链爱好者组成。

Terminet在分布式高性能计算、节点基础设施、网络、硬件、API支持和Web 3生态系统的存储等方面有多年技术积累，致力于帮助投资者和普通 Token 持有者从数字资产中获利。构建可靠、安全、易用的 web3 基础设施服务层，降低 web3 连接壁垒和 web3 应用开发门槛，为 web3 开发赋能。