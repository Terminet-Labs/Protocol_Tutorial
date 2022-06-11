## OasisAPI Rosetta Gateway

Oasis nodes are divided into validator node, non-validator node, and seed node. If you do not want to deploy nodes in the oasis network and just want to invoke RPC, Terminet opens a non-authenticated node for RPC using oasis-rosetta-gateway, which exposes a subset of the consensus layer via the [Rosetta API](https://www.rosetta-api.org/). 

**You can refer to the call method provided in this article to do so.**



**Welcome to delegate ROSE to Terminet**

>Validator: 0% Fee Terminet
>
>Validator Entity Address: oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66
>
>https://www.oasisscan.com/validators/detail/oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66



## Networks

​	Mainnet API：https://oasisapi.terminet.io

​	Testnet API：https://oasistestapi.terminet.io

## Simple Data API Example

For more usage reference：[oasis_rosetta_gateway](https://github.com/oasisprotocol/oasis-rosetta-gateway)

### 1. Get Network 

```yaml
 # Request
 curl -XPOST  https://oasistestapi.terminet.io/network/list --data '{"metadata": {}}'
 # Response
{
    "network_identifiers":[
        {
            "blockchain":"Oasis",
            "network":"50304f98ddb656620ea817cc1446c401752a05a249b36c9b90dba4616829977a"
        }
    ]
}
```

`network` string is the lowercase hex encoded SHA-512/256 hash of the CBOR encoded genesis document

### 2. Get Network Status

```bash
# Request
curl -XPOST https://oasistestapi.terminet.io/network/status --data \
'{
    "network_identifier": {
        "blockchain": "Oasis",
        "network": "50304f98ddb656620ea817cc1446c401752a05a249b36c9b90dba4616829977a"
    },
    "metadata": {}
}'
# Response
{
  "current_block_identifier": {
    "index": 9987562,
    "hash": "e0ad0aaa33226430d698fe849fcfba84eb60d45cb8296eba140e546d25c08d89"
  },
  "current_block_timestamp": 1654142157000,
  "genesis_block_identifier": {
    "index": 8535081,
    "hash": "5864fda0248a1fa53e65d36d9288ee89578003f8980d1900f76638737b43d0ee"
  },
  "oldest_block_identifier": {
    "index": 8535081,
    "hash": "5864fda0248a1fa53e65d36d9288ee89578003f8980d1900f76638737b43d0ee"
  },
  "peers": [
    {
      "peer_id": "183e1b068c74729a730b00a2a592c284e8a11069@167.71.39.73:26658"
    },
    {
      "peer_id": "f018d5d96c280ca888e4fd49df4494e68e80a2d9@65.108.215.176:26656"
    },
    {
      "peer_id": "726a6823bd231dae0dbdab6f443d0b0c01c3439f@176.9.16.60:29656"
    },
    {
      "peer_id": "7b85ea4901e90b0839a123e6a6043603a3d0de9f@20.80.34.71:26657"
    },
    {
      "peer_id": "49f568338f9137c734e5aa6c857e4ea79db6d663@65.52.244.40:26656"
    },
    {
      "peer_id": "7bcb02a07d161b5b576bd7b9963f39e3dbe1bfe8@94.130.136.77:36656"
    },
    {
      "peer_id": "2c68e7ca63117f5e1e6a7e2e1a1a36ce3ad73402@95.217.119.122:26656"
    },
    {
      "peer_id": "ebfc396480560c0a3fa2b82d9a96f17bdda9a3ed@146.59.68.8:34649"
    },
    {
      "peer_id": "96dd3a348ea08e4517c853ab7e1c1f8b68e76b36@217.160.225.11:56656"
    },
    {
      "peer_id": "676d5f8ae9c14a27a6de7cd6157f8e0523edec66@65.21.124.228:36657"
    },
    {
      "peer_id": "71504a0a83c25b48691ab708f45079248bab0c31@135.181.179.38:36656"
    },
    {
      "peer_id": "b2f26729d87471814624f096b1fb7f3312e4e13a@65.108.159.229:26656"
    },
    {
      "peer_id": "097b640924c0a8e8080e21cd537df2b01415c202@95.217.85.213:36656"
    },
    {
      "peer_id": "7187d60bb22c553646d44319b697be2c5c69eaf4@131.153.47.62:36656"
    },
    {
      "peer_id": "d4bfdb52a434d58e49fb160f61f8b0faa8439599@80.64.208.21:26656"
    },
    {
      "peer_id": "071ab61d3a222eb7026be3e71ed2568e60bdbf4a@94.130.34.15:34656"
    },
    {
      "peer_id": "3a17fbb5cf5dbc1e39406182baebdf71e7bd7063@135.181.112.38:26657"
    },
    {
      "peer_id": "f5b78775b81b80611c344432ac9deea95f3c564b@142.132.235.52:26656"
    },
    {
      "peer_id": "509d0687f51759c3043bc2cdcd776234ab56c9ac@95.216.72.47:31656"
    },
    {
      "peer_id": "15f8046124dc51f36acd01f5fab2bda66ca4fdc0@159.69.69.138:26756"
    }
  ]
}
```

### 3. Get an Account's Balance

```bash
# Request
curl -XPOST https://oasistestapi.terminet.io/account/balance --data \
'{
    "network_identifier": {
        "blockchain": "Oasis",
        "network": "50304f98ddb656620ea817cc1446c401752a05a249b36c9b90dba4616829977a"
    },
    "account_identifier": {
        "address": "oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4"
    },
    # 该设置可不填，返回最新块高时的账户余额
    "block_identifier": {
        "index": 9987698,
        "hash": "0a3af54098606ec9464426fcf751445498cb75a1c0c3743cf046e27b5c165758"
    },
    "currencies": [
        {
            "symbol": "ROSE",
            "decimals": 9
        }
    ]
}'
# Response
{
  "block_identifier": {
    "index": 9991192,
    "hash": "e2bc1f0ea3869aebbe881f90614bcbf3748e8fdc5a2ff4a2d8d63296bab270e8"
  },
  "balances": [
    {
      "value": "9999974869",
      "currency": {
        "symbol": "ROSE",
        "decimals": 9
      }
    }
  ],
  "metadata": {
    "nonce": 17
  }
}
```

###  4. Get a Block

```bash
# Request
curl -XPOST https://oasistestapi.terminet.io/block --data \
'{
    "network_identifier": {
        "blockchain": "Oasis",
        "network": "50304f98ddb656620ea817cc1446c401752a05a249b36c9b90dba4616829977a"
    },
    "block_identifier": {
        "index": 9987698
    }
}'
# Response
{
  "block": {
    "block_identifier": {
      "index": 9987698,
      "hash": "0a3af54098606ec9464426fcf751445498cb75a1c0c3743cf046e27b5c165758"
    },
    "parent_block_identifier": {
      "index": 9987697,
      "hash": "ca098e4e2717b80b169f152370b9d46b785cc3182a2aae983db12037264a9353"
    },
    "timestamp": 1654142915000,
    "transactions": [
      {
        "transaction_identifier": {
          "hash": "be5ceb1c6a20ead5f18348eceb311bcda0325a4177cfc1d210e647dd69ee2e8a"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "61dcc80af10f7ed851c74b1987f2ec0d1474652c8323ef4013c498afca7e3220"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "9236c0467f847b80376336c25df8718dcd8405d8e5a9fd8b9b697d0fa2c33c5e"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "341d208fb7e6cfbd56d7d256210b9f16af133299748e37c1e294fad5834fb8f9"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "5fe4a0949a17b5d3852e02b5aabceb00976b7dd6ea103fc2fccb27e97ad57793"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "962058e5e054929429edc258618069818c44fdcb335cc72e63a30ceec46e6ca3"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "8b4a49e84ff7a5c4d9bb07aa12c5f7391d88b8688a8f5079a6c07ebfa6c9488b"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "80ce5e0782cd01532a7c05470bbe9e93dfd84888cee6fab150c32e207c2a6bd2"
        },
        "operations": []
      },
      {
        "transaction_identifier": {
          "hash": "bc7333df197da8ddea37704dc7383a870bd99e0df4651c602378b79c57acbeda"
        },
        "operations": []
      }
    ],
    "metadata": {
      "epoch": 16630
    }
  }
}
```



---



If you have any questions, please feel free to reach out to us using our social media channels as below

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)

## About Terminet

Terminet is a professional Staking and ecosystem service provider. The core team consists of experienced developers, product and operations experts and blockchain enthusiasts with more than 5 years of experience in the blockchain industry.

Terminet is dedicated to helping investors and ordinary Token holders earn profits from digital assets. Build a reliable, secure and easy-to-use web3 infrastructure service layer to lower web3 connecting barriers and web3 application development thresholds, powering web3 development.
