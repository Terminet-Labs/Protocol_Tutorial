[TOC]

Stafi 是一种去中心化协议，赋予流动性。建立在 Substrate 之上的 Stafi 将作为平行链连接到 Polkadot，共享 Polkadot 的底层共识。

StaFi节点目前有主网和Seiya测试网，如果你不想部署节点，只是想简单的调用节点API，Terminet提供了一个非验证的RPC节点供社区开发者使用。



您可以参考本文档提供的API连接到StaFi协议，并通过相关调用方法查询链上数据。



**欢迎委托FIS 给Terminet验证人**

> 验证人名称: Terminet
>
> 验证人地址：33LC788z3YEJ54frTKvNYJKpPwDxB4r22uC7qzeftrbmscpN
>
> https://apps.stafi.io/#/staking



#### 网络

主网API：RPC：https://stafiapi.terminet.io/rpc

​				 WS：wss://stafiapi.terminet.io/ws

测试网API：RPC：https://stafitestapi.terminet.io

​					 WS：wss://stafitestapi.terminet.io/ws



#### 简单数据API示例

更多使用参数：https://polkadot.js.org/docs/substrate/rpc

##### 1、查看节点公开的rpc方法

```shell
# Request
$ curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "rpc_methods", "params":[]}' https://stafitestapi.terminet.io/rpc
# Response
{
  "jsonrpc": "2.0",
  "result": {
    "methods": [
      "account_nextIndex",
      "author_hasKey",
      "author_hasSessionKeys",
      "author_insertKey",
      "author_pendingExtrinsics",
      "author_removeExtrinsic",
      "author_rotateKeys",
      "author_submitAndWatchExtrinsic",
      "author_submitExtrinsic",
      "author_unwatchExtrinsic",
      "babe_epochAuthorship",
      "chain_getBlock",
      "chain_getBlockHash",
      "chain_getFinalisedHead",
      "chain_getFinalizedHead",
      "chain_getHead",
      "chain_getHeader",
      "chain_getRuntimeVersion",
      "chain_subscribeAllHeads",
      "chain_subscribeFinalisedHeads",
      "chain_subscribeFinalizedHeads",
      "chain_subscribeNewHead",
      "chain_subscribeNewHeads",
      "chain_subscribeRuntimeVersion",
      "chain_unsubscribeAllHeads",
      "chain_unsubscribeFinalisedHeads",
      "chain_unsubscribeFinalizedHeads",
      "chain_unsubscribeNewHead",
      "chain_unsubscribeNewHeads",
      "chain_unsubscribeRuntimeVersion",
      "childstate_getKeys",
      "childstate_getStorage",
      "childstate_getStorageHash",
      "childstate_getStorageSize",
      "grandpa_proveFinality",
      "grandpa_roundState",
      "grandpa_subscribeJustifications",
      "grandpa_unsubscribeJustifications",
      "offchain_localStorageGet",
      "offchain_localStorageSet",
      "payment_queryInfo",
      "state_call",
      "state_callAt",
      "state_getKeys",
      "state_getKeysPaged",
      "state_getKeysPagedAt",
      "state_getMetadata",
      "state_getPairs",
      "state_getReadProof",
      "state_getRuntimeVersion",
      "state_getStorage",
      "state_getStorageAt",
      "state_getStorageHash",
      "state_getStorageHashAt",
      "state_getStorageSize",
      "state_getStorageSizeAt",
      "state_queryStorage",
      "state_queryStorageAt",
      "state_subscribeRuntimeVersion",
      "state_subscribeStorage",
      "state_unsubscribeRuntimeVersion",
      "state_unsubscribeStorage",
      "subscribe_newHead",
      "system_accountNextIndex",
      "system_addReservedPeer",
      "system_chain",
      "system_chainType",
      "system_dryRun",
      "system_dryRunAt",
      "system_health",
      "system_localListenAddresses",
      "system_localPeerId",
      "system_name",
      "system_networkState",
      "system_nodeRoles",
      "system_peers",
      "system_properties",
      "system_removeReservedPeer",
      "system_version",
      "unsubscribe_newHead"
    ],
    "version": 1
  },
  "id": 1
}
```

##### 2、查看链信息

```shell
# request
$ curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "
system_chain", "params":[]}' https://stafitestapi.terminet.io/rpc
# Response
{
  "jsonrpc": "2.0",
  "result": "Stafi Testnet Seiya",
  "id": 1
}
```

##### 3、获取块高

获取的是当前节点的块高

```shell
# Request
$ curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "chain_getBlock", "params":[]}' https://stafitestapi.terminet.io/rpc
# Response
{
  "jsonrpc": "2.0",
  "result": {
    "block": {
      "extrinsics": [
        "0x280403000b6091d55b8101",
        "0x1c040f00e6e28200"
      ],
      "header": {
        "digest": {
          "logs": [
            "0x06424142453402020000001a25711000000000",
            "0x05424142450101f03b3ff8af3f69ddbb0577e15ec3aee35f4addd8f78318f0099b2c70e0d90653ae02e45bcbb3f919ddecfe837d1c0b5bccd89351e7356671c6548fbe2b42268a"
          ]
        },
        "extrinsicsRoot": "0xdf3ef20247de9578bfa09c89a4d9f1d191a888547a05bdb658d6a34701b2e533",
        "number": "0x20b8bc", # 该块高值为十六进制，需去掉0x转换为十进制
        "parentHash": "0x18c228be479711e297277f1717cb4ffbf20962f6cf32d68cb1753e1d5a3d1bde",
        "stateRoot": "0x9fccd5f745f2497e592bdeefd798ef8178b9d10842343ee397432405a04a45a5"
      }
    },
    "justification": null
  },
  "id": 1
}
```

##### 4、获取节点块高哈希

```shell
# Request
$ curl -H "Content-Type: application/json" https://stafitestapi.terminet.io/rpc -d \
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "chain_getBlockHash",
  "params": [
    "0x624ee1" # 需为十六进制格式
  ]
}
# Response
{
  "jsonrpc": "2.0",
  "result": "0x121cf366f016e44181ef806c6c494e1a9381c613e47b1bbed932a7111e05051e",
  "id": 1
}
```

##### 5、检查会话密钥是否存在

如果密钥库具有给定会话公钥的私钥，则返回true

```shell
# Request
$ curl -H "Content-Type: application/json" https://stafitestapi.terminet.io/rpc -d \
{
  "id": 1,
  "jsonrpc": "2.0",
  "method": "author_hasSessionKeys",
  "params": [
    "Key" # 自定义修改为想要检查的key
  ]
}
# Response
{
  "jsonrpc": "2.0",
  "result": true,
  "id": 1
}
```

##### 6、检索链类型

```shell
# Request
$ curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "system_chainType", "params":[]}' https://stafitestapi.terminet.io/rpc
# Response
{
  "jsonrpc": "2.0",
  "result": "Live",
  "id": 1
}
```

##### 7、查看节点的健康状态

```shell
# Request
$ curl -H "Content-Type: application/json" -d '{"id":1, "jsonrpc":"2.0", "method": "system_health", "params":[]}' https://stafitestapi.terminet.io/rpc
# Response
{
  "jsonrpc": "2.0",
  "result": {
    "isSyncing": true,
    "peers": 32,
    "shouldHavePeers": true
  },
  "id": 1
}
```

