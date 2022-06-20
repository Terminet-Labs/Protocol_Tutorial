# Terminet - Fetch.ai 节点API使用说明

Fetch.ai 是一个去中心化的区块链网络，提供支持基于 AI 的去中心化数字经济发展的工具和服务。它为用户提供网络、分布式账本、人工智能和机器学习等技术。个人和企业用户可以访问人工智能基础设施服务以及在网络内执行 DeFi 交易。

Fetchhub主网构成了Fetch.ai生态系统的核心, 如果你不想部署节点，只是想简单的调用节点API，Terminet提供了一个公开可靠的RPC节点供社区免费使用。



**欢迎委托IRIS 给Terminet验证人**

> 验证人名称: Terminet
>
> 验证人地址：fetchvaloper1k43xsdgf528dxp2fn27dq0l9297v4gjqk2heuu
>
> https://explore-fetchhub.fetch.ai/validator/fetchvaloper1k43xsdgf528dxp2fn27dq0l9297v4gjqk2heuu



## API配置

Fetch.ai主网API：https://fetchapi.terminet.io

##### 如下是可调用参数：

###### 参考链接：https://rpc-fetchhub.fetch.ai/

```shell
https://fetchapi.terminet.io/abci_info?
https://fetchapi.terminet.io/abci_query?path=_&data=_&height=_&prove=_
https://fetchapi.terminet.io/block?height=_
https://fetchapi.terminet.io/block_by_hash?hash= _
https://fetchapi.terminet.io/block_results?height=_
https://fetchapi.terminet.io/block_search?query=_&page=_&per_page=_&order_by=_
https://fetchapi.terminet.io/blockchain?minHeight =_&maxHeight=_
https://fetchapi.terminet.io/broadcast_evidence?evidence=_
https://fetchapi.terminet.io/broadcast_tx_async?tx=_
https://fetchapi.terminet.io/broadcast_tx_commit?tx=_
https://fetchapi.terminet.ioi/broadcast_tx_sync?tx=_
https://fetchapi.terminet.io/check_tx?tx=_
https://fetchapi.terminet.io/commit?height=_
https://fetchapi.terminet.io/consensus_params?height=_
https://fetchapi.terminet.io/consensus_state?
https://fetchapi.terminet.io/dump_consensus_state?
https://fetchapi.terminet.io/genesis?
https://fetchapi.terminet.io/genesis_chunked?chunk=_
https://fetchapi.terminet.io/health?
https://fetchapi.terminet.io/net_info?
https://fetchapi.terminet.io/num_unconfirmed_txs?
https://fetchapi.terminet.io/status?
https://fetchapi.terminet.io/subscribe?query=_
https://fetchapi.terminet.io/tx?hash=_&prove=_
https://fetchapi.terminet.io/tx_search?query=_&prove=_&page= _&per_page=_&order_by=_
https://fetchapi.terminet.io/unconfirmed_txs?limit=_
https://fetchapi.terminet.io/unsubscribe?query=_
https://fetchapi.terminet.io/unsubscribe_all?
https://fetchapi.terminet.io/validators?height=_&page=_&per_page=_
```

## 简单数据API示例

##### 1、查看节点网络状态

```shell
# Request
$ curl -s https://fetchapi.terminet.io/status
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
      "id": "0a6e43a9ceebc3c07891cf6849dbd2a0eccb6317",
      "listen_addr": "tcp://0.0.0.0:26656",
      "network": "fetchhub-4",
      "version": "0.34.16",
      "channels": "40202122233038606100",
      "moniker": "Terminetapi",
      "other": {
        "tx_index": "on",
        "rpc_address": "tcp://0.0.0.0:26657"
      }
    },
    "sync_info": {
      "latest_block_hash": "A36E730FC7CE5D2D42823C02804FECD5B0883E07673AD6FE5DEB1AB5AB48674D",
      "latest_app_hash": "90289A638CDE5557CC95D6948B4DFF9F16CD2AD9F3F62FEF7F11C65E4DA9F494",
      "latest_block_height": "6025819",
      "latest_block_time": "2022-05-26T14:05:11.859177719Z",
      "earliest_block_hash": "740986C7CA3030DCCA59EBEC39DC322232B348FF9678FF9DF060A5D7D28A0EBA",
      "earliest_app_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
      "earliest_block_height": "5300201",
      "earliest_block_time": "2022-04-05T16:00:00Z",
      "catching_up": true
    },
    "validator_info": {
      "address": "1AB458510E495D32E855CA7DA5EEB36FEEEA760B",
      "pub_key": {
        "type": "tendermint/PubKeyEd25519",
        "value": "jQMGH3YM+VVjgz3PPZA/drZzYUaQxT+7PNXX63J+zE4="
      },
      "voting_power": "0"
    }
  }
}
```

##### 2、查看某个块信息

```shell
# Request
$ curl -s https://fetchapi.terminet.io/block_results?height=6028277
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "height": "6028277",
    "txs_results": null,
    "begin_block_events": [
      {
        "type": "coin_received",
        "attributes": [
          {
            "key": "cmVjZWl2ZXI=",
            "value": "ZmV0Y2gxbTNoMzB3bHZzZjhsbHJ1eHRwdWtkdnN5MGttMmt1bThtdnd1OWg=",
            "index": true
          },
          {
            "key": "YW1vdW50",
            "value": "NTE3ODI3ODAyNTIzMTEyODQ0OWFmZXQ=",
            "index": true
          }
        ]
      }
    ]
    "end_block_events": null,
    "validator_updates": null,
    "consensus_param_updates": {
      "block": {
        "max_bytes": "200000",
        "max_gas": "2000000"
      },
      "evidence": {
        "max_age_num_blocks": "100000",
        "max_age_duration": "172800000000000",
        "max_bytes": "200000"
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
$ curl -s https://fetchapi.terminet.io/net_info
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "listening": true,
    "listeners": [
      "Listener(@)"
    ],
    "n_peers": "22",
    "peers": [
      {
        "node_info": {
          "protocol_version": {
            "p2p": "8",
            "block": "11",
            "app": "0"
          },
          "id": "3dc86a3d6b208fd494f50f83414d1f1dedb0af73",
          "listen_addr": "tcp://0.0.0.0:26656",
          "network": "fetchhub-4",
          "version": "0.34.19",
          "channels": "40202122233038606100",
          "moniker": "forbole-sentry-2",
          "other": {
            "tx_index": "on",
            "rpc_address": "tcp://0.0.0.0:26657"
          }
        },
        "is_outbound": true,
        "connection_status": {
          "Duration": "1507961804734",
          "SendMonitor": {
            "Start": "2022-06-16T06:54:16.06Z",
            "Bytes": "71462",
            "Samples": "3696",
            "InstRate": "28",
            "CurRate": "28",
            "AvgRate": "47",
            "PeakRate": "3040",
            "BytesRem": "0",
            "Duration": "1507960000000",
            "Idle": "580000000",
            "TimeRem": "0",
            "Progress": 0,
            "Active": true
          },
          "RecvMonitor": {
            "Start": "2022-06-16T06:54:16.06Z",
            "Bytes": "31588186",
            "Samples": "5514",
            "InstRate": "127670",
            "CurRate": "40273",
            "AvgRate": "20936",
            "PeakRate": "1307710",
            "BytesRem": "0",
            "Duration": "1529220000000",
            "Idle": "100000000",
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
        "remote_ip": "65.21.229.209"
      }
    ]
  }
```

##### 4、查看某个块commit信息

```shell
# Request
$ curl -s https://fetchapi.terminet.io/commit?height=6043662
# response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "signed_header": {
      "header": {
        "version": {
          "block": "11"
        },
        "chain_id": "fetchhub-4",
        "height": "6043662",
        "time": "2022-05-27T20:17:42.821846759Z",
        "last_block_id": {
          "hash": "7972EF9866CF527DC37F39CE1FF1C6A416237B3D98120A7DAD183FCE666A0943",
          "parts": {
            "total": 1,
            "hash": "93C1B765E4E0FC6A1E01F8431412970058CD099BECD91DFFCAC2261D68B2E87A"
          }
        },
        "last_commit_hash": "FFFF47EF87BF912EA1555B557CACF7396C26630DD96870526B8A6D7269BB6EFB",
        "data_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "validators_hash": "17EFCB18142F9859824B085B6D467AC21AAF335EB25424C8560918DAC2BC9CB9",
        "next_validators_hash": "17EFCB18142F9859824B085B6D467AC21AAF335EB25424C8560918DAC2BC9CB9",
        "consensus_hash": "0F2908883A105C793B74495EB7D6DF2EEA479ED7FC9349206A65CB0F9987A0B8",
        "app_hash": "02B068B7ABEE4C6045F302482E68D129DB1DDF49FFFB61C93FDF50624A36EBF8",
        "last_results_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "evidence_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "proposer_address": "D7E025DD7943C57E521EA9D5E86A297D6D1E4E40"
      },
      "commit": {
        "height": "6043662",
        "round": 0,
        "block_id": {
          "hash": "D1BB04FFEC49AE506859555C93F0FD301323965EF332468219FB57BDE0E593FE",
          "parts": {
            "total": 1,
            "hash": "DF7CA8EA3C86FB60844EC99018EF4F2A323F8DE4EC853D4DCD369794D317227F"
          }
        },
        "signatures": [
          {
            "block_id_flag": 2,
            "validator_address": "72166E33E41D5BD501213D9FEAA336626A0C21A6",
            "timestamp": "2022-05-27T20:17:49.051288817Z",
            "signature": "X8dG/ziTjTvbu/rWryj4sbB1DCxkmCNkVzIuM766Lb+vb38u7diqDaaybN+D9+VcDgA8d6feZI/ZNOAeoBVdAQ=="
          },
          {
            "block_id_flag": 2,
            "validator_address": "7E3D3E3F65AC31EC1140BA88CC1973A79B6E586F",
            "timestamp": "2022-05-27T20:17:48.860820559Z",
            "signature": "K6GYPE2khth983wRNkI1ljAiz4hL5G2YmV13XPkoPEzSrsgeNS+8H74868YYdHFwdAxzoH5sTjpiCOlHQfiqAA=="
          },
          {
            "block_id_flag": 2,
            "validator_address": "733049D7FC042813107153018C6D0976593ABB05",
            "timestamp": "2022-05-27T20:17:49.162496417Z",
            "signature": "4kSJDFyNp70padaWnJVDu5iIDATZKOzVEAP9AMYJGxynKvA9imTrvhW0EOcsx/vJFABZfc5CDjF1xjbsRuglBw=="
          }
        ]
      }
    },
    "canonical": true
  }
}
```

##### 5、查看某个块hash信息

```shell
# Request
$ curl -s https://fetchapi.terminet.io/block_by_hash?hash=0x7972EF9866CF527DC37F39CE1FF1C6A416237B3D98120A7DAD183FCE666A0943
# Response
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "block_id": {
      "hash": "7972EF9866CF527DC37F39CE1FF1C6A416237B3D98120A7DAD183FCE666A0943",
      "parts": {
        "total": 1,
        "hash": "93C1B765E4E0FC6A1E01F8431412970058CD099BECD91DFFCAC2261D68B2E87A"
      }
    },
    "block": {
      "header": {
        "version": {
          "block": "11"
        },
        "chain_id": "fetchhub-4",
        "height": "6043661",
        "time": "2022-05-27T20:17:36.896741516Z",
        "last_block_id": {
          "hash": "28ED1CA7E62E8A059534F764FEB01D46C2C9B9BFFD38819731C096DA01B6732B",
          "parts": {
            "total": 1,
            "hash": "684D00D7E868A43D56E721A965B4E95802F26616FF49D2E2BF89A133ED4D4B71"
          }
        },
        "last_commit_hash": "A5E7B00C7BBBA32D18C870649D5A89E9C3A09EC52ADB64E902CBB3F5A5606C72",
        "data_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "validators_hash": "17EFCB18142F9859824B085B6D467AC21AAF335EB25424C8560918DAC2BC9CB9",
        "next_validators_hash": "17EFCB18142F9859824B085B6D467AC21AAF335EB25424C8560918DAC2BC9CB9",
        "consensus_hash": "0F2908883A105C793B74495EB7D6DF2EEA479ED7FC9349206A65CB0F9987A0B8",
        "app_hash": "22538F38A32D79754081F5402F79AD928D96442CE6C07ABD02EC10555E23C5EF",
        "last_results_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "evidence_hash": "E3B0C44298FC1C149AFBF4C8996FB92427AE41E4649B934CA495991B7852B855",
        "proposer_address": "629C5DEFC2372165D12C3ED723EA86116E68CF0E"
      },
      "data": {
        "txs": []
      },
      "evidence": {
        "evidence": []
      },
      "last_commit": {
        "height": "6043660",
        "round": 0,
        "block_id": {
          "hash": "28ED1CA7E62E8A059534F764FEB01D46C2C9B9BFFD38819731C096DA01B6732B",
          "parts": {
            "total": 1,
            "hash": "684D00D7E868A43D56E721A965B4E95802F26616FF49D2E2BF89A133ED4D4B71"
          }
        },
        "signatures": [
          {
            "block_id_flag": 2,
            "validator_address": "72166E33E41D5BD501213D9FEAA336626A0C21A6",
            "timestamp": "2022-05-27T20:17:37.068393394Z",
            "signature": "ZKIUQcUGvI4ZDZNNLRnXXTFjjNvn3+BLu2R+hPNeZvsPGbHtxDaxry/y19PWh8UIzTHHKg7COG1nQXC0Fx+jDQ=="
          },
          {
            "block_id_flag": 2,
            "validator_address": "602FB24FCA975632E8A1C452468DF626E524D51B",
            "timestamp": "2022-05-27T20:17:36.896741516Z",
            "signature": "5/HkO2iFdT4kXm0ZAZ7dNQkSe55PJqiaQmsbYIbzQzL6gZnRXeUwJJjQneZOSYP+bzZkhKOI9w//ZLJ5scliCA=="
          },
          {
            "block_id_flag": 2,
            "validator_address": "733049D7FC042813107153018C6D0976593ABB05",
            "timestamp": "2022-05-27T20:17:37.168680693Z",
            "signature": "cvTr/ozntUGXL0jg6N2PztkpOpWBRMU0RSgWJqhPKCRNURpAm6iCOslzAUXpFUNMcWWXzoztoXzLOL5BxuofBQ=="
          }
        ]
      }
    }
  }
}
```

##### 6、查看某个块验证者信息

```shell
# Request
$ curl -s https://fetchapi.terminet.io/validators?height=6069541
{
  "jsonrpc": "2.0",
  "id": -1,
  "result": {
    "block_height": "6069541",
    "validators": [
      {
        "address": "72166E33E41D5BD501213D9FEAA336626A0C21A6",
        "pub_key": {
          "type": "tendermint/PubKeyEd25519",
          "value": "x9g0/4fPYkPtNcaeef48tVqWPi/OR25alwZPL7odDJU="
        },
        "voting_power": "27485457",
        "proposer_priority": "91875817"
      },
      {
        "address": "602FB24FCA975632E8A1C452468DF626E524D51B",
        "pub_key": {
          "type": "tendermint/PubKeyEd25519",
          "value": "HaFP16YPfmep+eddKGX7sIS9VuoZOI3SQhCpqsWRMNM="
        },
        "voting_power": "24769027",
        "proposer_priority": "107961610"
      },
      {
        "address": "57EF42F7B7A2DF887C4ED687636CF86B158D0D64",
        "pub_key": {
          "type": "tendermint/PubKeyEd25519",
          "value": "42Md7e++cwBFnRs61e59QQYWd0QxJ9FlO5NQfTG+1Lk="
        },
        "voting_power": "3070374",
        "proposer_priority": "78002722"
      },
      {
        "address": "E5937BCF34FD9BAA4430F34BDF86180A0BCAB901",
        "pub_key": {
          "type": "tendermint/PubKeyEd25519",
          "value": "Uq8vs54lsKeBxbGotuYspZ/8ponQR/pIiEwbw0LeUsk="
        },
        "voting_power": "3032101",
        "proposer_priority": "-35120013"
      }
    ],
    "count": "30",
    "total": "60"
  }
}
```

---



如果您有任何问题，请随时使用我们的社交媒体渠道与我们联系，联系方式如下：

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)



## 关于Terminet

Terminet 是一家专业的 Staking 和生态系统服务提供商。核心团队由经验丰富的开发人员、产品和运营专家以及区块链行业5年以上经验的区块链爱好者组成。

Terminet在分布式高性能计算、节点基础设施、网络、硬件、API支持和Web 3生态系统的存储等方面有多年技术积累，致力于帮助投资者和普通 Token 持有者从数字资产中获利。构建可靠、安全、易用的 web3 基础设施服务层，降低 web3 连接壁垒和 web3 应用开发门槛，为 web3 开发赋能。