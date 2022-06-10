[toc]

Terminet已在Oasis网络部署验证者已经有一段时间了，虽然官方有验证人文档，但是整体比较分散，存在一些与实际操作流程或者真实环境不匹配的情况。

Terminet结合自己实际的节点运维经验，将验证节点部署（包括质押）和节点运维操作（迁移和监控）的整体流程和资料进行汇总提炼和总结，以简洁清晰的方式整理本指导教程供社区参考。

如果你对Oasis network感兴趣，想运行一个validator节点，本文档希望能给到你一些帮助。



**欢迎委托ROSE给Terminet验证人**

> 验证人名称: 0% Fee Terminet
>
> 验证人Entity Address: oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66
>
> https://www.oasisscan.com/validators/detail/oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66



#### 资源导航

官网：https://zh.oasisprotocol.org/node

官方文档：https://docs.oasis.dev/general/oasis-network/network-parameters

Oasis浏览器插件钱包（支持主网和测试网）：https://docs.oasis.dev/general/manage-tokens/oasis-wallets/browser-extension

网页版钱包（支持主网和测试网）：https://wallet.oasisprotocol.org

浏览器（支持主网和测试网）：https://www.oasisscan.com

测试网水龙头：https://faucet.testnet.oasis.dev

#### 主机硬件要求

 [官方详细参考](https://docs.oasis.dev/general/run-a-node/prerequisites/hardware-recommendations)

| 服务器                                              | 用途                                                         |
| --------------------------------------------------- | ------------------------------------------------------------ |
| Ubuntu18.04、1核CPU、2G内存、50G系统盘              | localhost操作（无需网络，用于离线生成文件，提供给server部署节点） |
| Ubuntu18.04、4核CPU、8G内存、100G系统盘、300G数据盘 | server操作                                                   |

#### 主机初始化

1. 若有数据盘,在/opt上挂载

/dev/vdb需要修改成实际盘名称

```bash
sudo parted /dev/vdb mklabel gpt
sudo parted  /dev/vdb mkpart primary 0% 100%
sudo mkfs.ext4 /dev/vdb1
echo "/opt /dev/vdb1     ext4    defaults,discard 0 0" |sudo tee -a /etc/fstab
sudo mount -a
```

2. 创建一个普通用户

运行验证器节点和质押都使用该用户登陆操作

```bash
sudo useradd --system oasis --shell /usr/sbin/login
echo "oasis ALL=(ALL) ALL" |sudo tee -a /etc/sudoers.d/oasis
```

3. 增大文件描述符

```bash
# 以root用户执行以下命令
cat > /etc/security/limits.d/99-oasis-node.conf << EOF
*        soft    nofile    102400
*        hard    nofile    1048576
EOF
# 查看值,若是1048576则已生效，若不是需要重启主机生效
ulimit -n
```

#### 运行验证器节点

加入主网和测试网的操作步骤一样，其中genesis文件和种子节点不一样，oasis二进制可能一样，若有更新则可参考该链接：https://docs.oasis.dev/general/oasis-network/network-parameters

为了保证密钥文件安全性，localhost操作和server操作可在不同服务器上执行

##### localhost操作

初始化实体和验证人，创建部署server节点所需的密钥和其他文件，可在离线服务器上操作（前提是机器上有oasis-node二进制）

1. 创建目录结构

```bash
mkdir /opt/oasis_localhost/
sudo chown oasis.oasis /opt/oasis_localhost
cd /opt/oasis_localhost/
mkdir -m700 -p {entity,node}
```

2. 下载oasis-node二进制

```shell
wget https://github.com/oasisprotocol/oasis-core/releases/download/v22.1.3/oasis_core_22.1.3_linux_amd64.tar.gz
tar -xf oasis_core_22.1.3_linux_amd64.tar.gz
sudo mv oasis_core_22.1.3_linux_amd64/oasis-node /usr/bin/

# 检查版本
oasis-node --version
```

3. 下载创世文件，主网和测试网二选一

```bash
# 主网
wget https://github.com/oasisprotocol/mainnet-artifacts/releases/download/2022-04-11/genesis.json -P /opt/oasis_localhost
# 测试网
wget https://github.com/oasisprotocol/testnet-artifacts/releases/download/2022-03-03/genesis.json -P /opt/oasis_localhost
```

4. 初始化实体和节点

```bash
# 设置创世文件环境变量
GENESIS_FILE_PATH=/opt/oasis_localhost/genesis.json

# 初始化实体，基于file的操作
cd /opt/oasis_localhost/entity 
oasis-node registry entity init

# 获取id,填写到ENTITY_ID
cat entity.json
{
  "v": 2,
  "id": "ChqSTUlQol8IzTse7PrDa+uGIQqRZFycFrGgIbk5YHk="
}

# 初始化节点
cd /opt/oasis_localhost/node
ENTITY_ID=ChqSTUlQol8IzTse7PrDa+uGIQqRZFycFrGgIbk5YHk=
# 这个STATIC_IP是你的主机的公网IP
STATIC_IP=8.219.49.103
oasis-node registry node init \
  --node.entity_id $ENTITY_ID \
  --node.consensus_address $STATIC_IP:26656 \
  --node.role validator

#  将节点添加到实体
cd /opt/oasis_localhost/entity
# 这将更新实体描述符entity.json，随后更新entity_genesis.json包含签名实体描述符有效负载的文件。
oasis-node registry entity update \
  --entity.node.descriptor /opt/oasis_localhost/node/node_genesis.json
 
```

##### server操作

1. 创建目录结构

```
mkdir -P /opt/oasis_server/log
sudo chown oasis.oasis /opt/oasis_server/ 
mkdir -m700 -p /opt/oasis_server/{etc,node,node/entity}
```

2. 复制实体和节点文件到server主机，下载genesis.json创世文件

> 将/opt/oasis_localhost/node目录下的文件上传到/opt/oasis_server/node，具体文件如下
>
> consensus.pem
> consensus_pub.pem
> identity.pem
> identity_pub.pem
> p2p.pem
> p2p_pub.pem
> sentry_client_tls_identity.pem
> sentry_client_tls_identity_cert.pem
>
> 并确保所有这些文件都有0600权限
>
> chmod -R 600 /opt/oasis_server/node/*.pem
>
> 将 /opt/oasis_localhost/entity/entity.json上传到 /opt/oasis_server/node/entity
>
> 将/opt/oasis_localhost/genesis.json上传到 /opt/oasis_server/etc/

3. 创建config.yml文件，按需求下列参数

   {{ external_address }} 替换为服务器公网IP）

   测试网设置

   {{ seed_node_address }} 替换为参数：05EAC99BB37F6DAAD4B13386FF5E087ACBDDC450@34.86.165.6:26656

   主网设置

   {{ seed_node_address }} 替换为参数： E27F6B7A350B4CC2B48A6CBE94B0A02B0DCB0BF3@35.199.49.168:26656

```bash
cat > /opt/oasis_server/etc/config.yml << EOF
##
# Oasis Node Configuration
#
# This file's contents are derived from the command line arguments found in the
# root command of the oasis-node binary. For more information, execute:
#
#     oasis-node --help
#
# Settings on the command line that are separated by a dot all belong to the
# same nested object. For example, "--foo.bar.baz hello" would translate to:
#
#     foo:
#       bar:
#         baz: hello
##

# Set this to where you wish to store node data. The node's artifacts
# should also be located in this directory.
datadir: /opt/oasis_server/node

# Logging.
#
# Per-module log levels are defined below. If you prefer just one unified log
# level, you can use:
#
# log:
#   level: debug
log:
  level:
    # Per-module log levels. Longest prefix match will be taken. Fallback to
    # "default", if no match.
    default: debug
    tendermint: warn
    tendermint/context: error
  format: JSON
  # By default logs are output to stdout. If you would like to output logs to
  # a file, you can use:
  #
  file: /opt/oasis_server/oasis-node.log

# Genesis.
genesis:
  # Path to the genesis file for the current version of the network.
  file: /opt/oasis_server/etc/genesis.json

# Worker configuration.
worker:
  registration:
    # In order for the node to register itself, the entity.json of the entity
    # used to provision the node must be available on the node.
    entity: /opt/oasis_server/node/entity/entity.json

# Consensus backend.
consensus:
  # Setting this to true will mean that the node you're deploying will attempt
  # to register as a validator.
  validator: true

  # Tendermint backend configuration.
  tendermint:
    core:
      listen_address: tcp://0.0.0.0:26656

      # The external IP that is used when registering this node to the network.
      # NOTE: If you are using the Sentry node setup, this option should be
      # omitted.
      external_address: tcp://{{ external_address }} :26656

    # List of seed nodes to connect to.
    # NOTE: You can add additional seed nodes to this list if you want.
    p2p:
      seed:
        - "{{ seed_node_address }}"
EOF
```

4. 启动节点

```bash
nohup oasis-node --config /opt/oasis_server/etc/config.yml 2>&1 > /dev/null &
```

5. 查看节点同步状态

```bash
oasis-node registry entity list -a unix:/opt/oasis_server/node/internal.sock
输入内容如下：
gb8SHLeDc69Elk7OTfqhtVgE2sqxrBCDQI84xKR+Bjg=
LaKJUKndAqH57DVExspTjdRTRNGJOSqArzM0pm8REZw=
DcplOZzEDthLHtPyjbtn9nldfzZ5b9r8EJFnD3JTGMI=
.....省略
```

6. 查看节点是否完成同步区块

```bash
# 完成区块同步将得到：'node completed initial syncing' 。 
oasis-node control is-synced -a unix:/opt/oasis_server/node/internal.sock
```

#### 质押

[官网质押提示](https://docs.oasis.dev/general/manage-tokens/advanced/oasis-cli-tools/common-staking-info)

##### localhost操作

1. 查看实体ID的质押账户地址 

```shell
# 查看实体ID即publickey
cat /opt/oasis_localhost/entity/entity.json
输出内容信息：
{
  "v": 2,
  "id": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
  "nodes": [
    "jzecOLosQppjlkjjkkt4FcdR2TCGpiFLgQLT1j5Gx5Q="
  ]
 }
# 将实体ID（Base64编码）转换为质押账户地址
$ oasis-node stake pubkey2address --public_key KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=
oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4 # 质押地址
```

2. 质押生成交易签名命令（质押门槛200）

```bash
# GENESIS_FILE_PATH是localhost上的创世文件：/opt/oasis_localhost/genesis.json
# ENTITY_DIR_PATH是localhost上entity所在路径：/opt/oasis_localho/entity/
# ACCOUNT_ADDRESS是账户质押地址，由entity ID转换得到: oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4
# OUTPUT_TX_FILE_PATH是签名文件输出路径：/opt/oasis_localhost/signed-escrow.tx
oasis-node stake account gen_escrow \
  --genesis.file $GENESIS_FILE_PATH \
  --signer.backend file \
  --signer.dir $ENTITY_DIR_PATH \
  --stake.escrow.account $ACCOUNT_ADDRESS \
  --stake.amount 200000000000 \
  --transaction.file $OUTPUT_TX_FILE_PATH \
  --transaction.fee.gas 2269 \
  --transaction.fee.amount 2000 \
  --transaction.nonce 0
You are about to sign the following transaction:
  Method: staking.AddEscrow
  Body:
    To:     oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4
    Amount: 200.0 TEST
  Nonce:  0
  Fee:
    Amount: 0.000002 TEST
    Gas limit: 2269
    (gas price: 0.0 TEST per gas unit)
Other info:
  Genesis document's hash: 50304f98ddb656620ea817cc1446c401752a05a249b36c9b90dba4616829977a

Are you sure you want to continue? (y)es/(n)o: y  

# 生成文件$OUTPUT_TX_FILE_PATH路径下：signed-escrow.tx
$ cat signed-escrow.tx
{
  "untrusted_raw_value": "pGNmZWWiY2dhcxkI3WZhbW91bnRCB9BkYm9keaJmYW1vdW50RS6Q7dAAZ2FjY291bnRVAFFqdv9gy+HL2vlz6iXZa510KoIwZW5vbmNlAGZtZXRob2Rxc3Rha2luZy5BZGRFc2Nyb3c=",
  "signature": {
    "public_key": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
    "signature": "vHg76Qq7qDV3QtkmNJbzh6S3aHPrNgOoXs0FNNx3scc5oVXYyFhFwlWZfTGUaerseCMli+hk6b4DWbY3FGfwCQ=="
  }
```

3. 生成实体注册交易签名命令

```shell
# GENESIS_FILE_PATH是localhost上的创世文件：/opt/oasis_localhost/genesis.json
# ENTITY_DIR_PATH是localhost上entity所在路径：/opt/oasis_localhost/entity/
# OUTPUT_REGISTER_TX_FILE_PATH是签名文件输出路径：/opt/oasis_localhost/signed-register.tx
oasis-node registry entity gen_register \
  --genesis.file $GENESIS_FILE_PATH \
  --signer.backend file \
  --signer.dir $ENTITY_DIR_PATH \
  --transaction.file $OUTPUT_REGISTER_TX_FILE_PATH \
  --transaction.fee.gas 2460 \
  --transaction.fee.amount 1000 \
  --transaction.nonce 1
You are about to sign the following transaction:
  Method: registry.RegisterEntity
  Body:
    {
      "untrusted_raw_value": {
        "v": 2,
        "id": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
        "nodes": [
          "jzecOLosQppjlkjjkkt4FcdR2TCGpiFLgQLT1j5Gx5Q="
        ]
      },
      "signature": {
        "public_key": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
        "signature": "QrtxGlPCpS+jO3fmL4vt9JpURxBaBKd6EQlKgQj3Jl2xIGATYPQvjH1gVwwLQRaqA6pLzFi7wQ21okSti7WYDQ=="
      }
    }
  Nonce:  1
  Fee:
    Amount: 0.000001 TEST
    Gas limit: 2460
    (gas price: 0.0 TEST per gas unit)
Other info:
  Genesis document's hash: 50304f98ddb656620ea817cc1446c401752a05a249b36c9b90dba4616829977a

Are you sure you want to continue? (y)es/(n)o: y

# 生成注册签名文件signed-register.tx，下面举例显示
$ cat signed-register.tx
{
  "untrusted_raw_value": "pGNmZWWiY2dhcxkJnGZhbW91bnRCA+hkYm9keaJpc2lnbmF0dXJlomlzaWduYXR1cmVYQEK7cRpTwqUvozt35i+L7fSaVEcQWgSnehEJSoEI9yZdsSBgE2D0L4x9YFcMC0EWqgOqS8xYu8ENtaJErYu1mA1qcHVibGljX2tleVggKzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcZzdW50cnVzdGVkX3Jhd192YWx1ZVhSo2F2AmJpZFggKzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcZlbm9kZXOBWCCPN5w4uixCmmOWSOOSS3gVx1HZMIamIUuBAtPWPkbHlGVub25jZQFmbWV0aG9kd3JlZ2lzdHJ5LlJlZ2lzdGVyRW50aXR5",
  "signature": {
    "public_key": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
    "signature": "Qrrx1vo0cwsL0g7+9oyBDqsO6GMKYZRfxZFEf3I/Iucc6w4gj2g2eCqjTKXn4RYvjzONxmHEQf90eTnSXJO6AQ=="
  }
```

##### server操作

将生成的两个签名文件复制到server目录下并提交

>将signed-escrow.tx和signed-register.tx上传到/opt/oasis_server

```shell
# 提交两个事务
oasis-node consensus submit_tx --transaction.file /opt/oasis_server/signed-escrow.tx -a unix:/serverdir/node/internal.sock

oasis-node consensus submit_tx --transaction.file /opt/oasis_server/signed-register.tx -a unix:/serverdir/node/internal.sock
```


#### 迁移验证者节点操作

> 说明：停原节点，将节点目录打包到新节点重启即可；oasis节点短时间离线不会有其他影响，离线超过一小时会退出活跃集，待重启后会恢复加入活跃集。

1、在新主机上安装二进制

```shell
wget https://github.com/oasisprotocol/oasis-core/releases/download/v22.1.7/oasis_core_22.1.7_linux_amd64.tar.gz
tar -xf oasis_core_22.1.7_linux_amd64.tar.gz
cd oasis_core_22.1.3_linux_amd64/
sudo mv oasis-node /usr/bin
# 检查版本
$ oasis-node --version
Software version: 22.1.7
Consensus:
  Consensus protocol version: 6.0.0
Runtime:
  Host protocol version:      5.0.0
  Committee protocol version: 4.0.0
Go toolchain version: 1.17.10
```

2、停原节点进程，将节点数据拷贝到新节点

```shell
# 原节点上操作
tar -czf oasis.tar.gz oasis_server/
scp oasis.tar.gz user@ip:/opt/

# 新主机上操作
tar -xf oasis.tar.gz

# 修改配置文件参数（注意其他文件路径是否有更改，比如日志）
vim /opt/oasis_server/etc/config.yml
external_address: tcp://新主机公网IP:26656
```

3、重启节点

```shell
nohup oasis-node --config /opt/oasis_server/etc/config.yml 2>&1 > oasis-node.log &
```

4、检查节点是否正常运行

```shell
# 查询节点网络情况,如有响应为链接成功，收到非零退出代码则未链接到网络。
$ oasis-node registry entity list -a unix:/opt/oasis_server/node/internal.sock
gb8SHLeDc69Elk7OTfqhtVgE2sqxrBCDQI84xKR+Bjg=
LaKJUKndAqH57DVExspTjdRTRNGJOSqArzM0pm8REZw=
DcplOZzEDthLHtPyjbtn9nldfzZ5b9r8EJFnD3JTGMI=
.....省略
# 检查节点是否同步完成，同步完成将得到：'node completed initial syncing' 。 
$ oasis-node control is-synced -a unix:/opt/oasis_server/node/internal.sock
$ tail -f oasis-node.log  |grep block_hash
# 查看同步快高
{"block_hash":"e9cc7941a246c7079180aa6cd083fa0ca65e25e5e6c121761328e918a9ec4603","block_height":8115558,"caller":"mux.go:818","last_retained_version":0,"level":"debug","module":"abci-mux","msg":"Commit","ts":"2022-04-22T08:22:19.767526073Z"}
```


#### 其他操作

- 查看节点当前区块高度

  ```bash
  oasis-node control status -a unix:/opt/oasis_server/node/internal.sock
  ```

- 查看网络的质押信息

  ```bash
  $ oasis-node stake info -a unix:/opt/oasis_server/node/internal.sock
  # 代币名称ROSE
  Token's ticker symbol: ROSE 
  # 
  Token's value base-10 exponent: 9
  # 总发行量 100亿
  Total supply: 10000000000.0 ROSE
  # 公共池: 13亿多
  Common pool: 1314419522.946253371 ROSE
  Last block fees: 0.0 ROSE
  Governance deposits: 0.0 ROSE
  # 实体的质押阈值100ROSE
  Staking threshold (entity): 100.0 ROSE
  # 验证人的质押阈值100ROSE
  Staking threshold (node-validator): 100.0 ROSE
  # 计算节点的质押阈值100ROSE
  Staking threshold (node-compute): 100.0 ROSE
  # 密钥管理器的质押阈值100ROSE
  Staking threshold (node-keymanager): 100.0 ROSE
  # 运行时计算的质押阈值50000ROSE
  Staking threshold (runtime-compute): 50000.0 ROSE
  # 运行时密钥管理器的质押阈值50000ROSE
  Staking threshold (runtime-keymanager): 50000.0 ROSE
  ```
  
- 获取 Oasis 节点公开的 gRPC

  ```bash
  oasis-node registry node list -v -a unix:/opt/oasis_server/node/internal.sock |   jq 'select(.roles | contains("consensus-rpc")) | .tls.addresses'
  ```

- 查看佣金计划规则

  ```bash
  cat /opt/oasis_localhost/genesis.json | \
  python3 -c 'import sys, json; \
  rules = json.load(sys.stdin)["staking"]["params"]["commission_schedule_rules"]; \
  print(json.dumps(rules, indent=4))'
  
  输出内容信息：
  # rate_change_interval：可以在每个epoch上更改佣金率
  # rate_bound_lead：必须提前至少336个epoch提交佣金率，用来设置修改佣金率中参数start_epoch的值
  {
      "rate_change_interval": 1,
      "rate_bound_lead": 336,
      "max_rate_steps": 10,
      "max_bound_steps": 10
  }
  ```

- 修改佣金率

  localhost操作

  ```bash
  # stake.commission_schedule.bounds：佣金率范围设置，格式“start_epoch/rate_min_numerator/rate_max_numerator”，比率都是numerator/100000，即范围0%～25%；start_epoch即设置范围从13738开始，参考浏览器当前epoch值13737，不得小于或等于当前epoch，且范围不超过当前epoch+336
  # stake.commission_schedule.rates：佣金率，格式“start_epoch/rate_numerator”，比率是rate_numerator/100000
  oasis-node stake account gen_amend_commission_schedule \
  "${TX_FLAGS[@]}" \
  --stake.commission_schedule.bounds 13738/0/25000 \
  --stake.commission_schedule.rates 13738/10000 \
  --transaction.file tx_amend_commission_schedule.json \
  --transaction.nonce 10 \
  --transaction.fee.gas 1297 \
  --transaction.fee.amount 2000 \
  --genesis.file /opt/oasis_localhost/genesis.json --signer.backend file --signer.dir /opt/oasis_localhost/entity/
  
  ```

  server操作

  ```bash
  oasis-node consensus submit_tx --transaction.file /opt/oasis_server/tx_amend_commission_schedule.json -a unix:/serverdir/node/internal.sock
  ```

- 查看最小委托数量

  server操作

  ```bash
  # 即最小委托数量为100个代币
  cat /opt/oasis_localhost/genesis.json | \
  python3 -c 'import sys, json; \
  print(json.dumps(json.load(sys.stdin)["staking"]["params"]["min_delegation"], indent=4))'
  
  输出内容信息：
  "100000000000"
  ```

- 回收委托/质押的代币

  server操作

  查看账户质押委托信息举例

  ```bash
  oasis-node stake account info --stake.account.address <质押账户地址> -a unix:/opt/oasis_server/node/internal.sock
  输出内容信息：
  Account State for Height: 9379363
  Balance:
    Total: 210.041343044 TEST
    Available: 9.999995 TEST
    Active Delegations from this Account:
      Total: 200.041348044 TEST
      Delegations:
        - To:     oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4 (self)
          Amount: 200.041348044 TEST (200000000000 shares) 
  
  Active Delegations to this Account:
    Total: 300.062022066 TEST (300000000000 shares)
    Delegations:
      - From:   oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4 (self)
        Amount: 200.041348044 TEST (200000000000 shares) # 根据质押金额选择括号中需要回收的shares数量
      - From:   oasis1qr3253pk6f0h0ek7a0ry03yhxc6c8zlt9vswwapa
        Amount: 100.020674022 TEST (100000000000 shares)
  
  Stake Accumulator:
    Claims:
      - Name: registry.RegisterEntity
        Staking Thresholds:
          - Global: entity
      - Name: registry.RegisterNode.jzecOLosQppjlkjjkkt4FcdR2TCGpiFLgQLT1j5Gx5Q=
        Staking Thresholds:
          - Global: node-validator
  
  Nonce: 3
  ```

  localhost操作

  选择回收的代币量生成交易（执行命令中数值为股权数shares)

  ```bash
  # stake.shares：回收选择的股权数
  # transaction.file：输出的交易签名文件
  oasis-node stake account gen_reclaim_escrow \
    "${TX_FLAGS[@]}" \
    --stake.shares 200000000000 \
    --stake.escrow.account <账户地址> \
    --transaction.file tx_reclaim.json \
    --transaction.nonce 3 \
    --transaction.fee.gas 1000 \
    --transaction.fee.amount 2000
  ```

  server操作

  提交交易

  ```bash
  oasis-node consensus submit_tx --transaction.file /opt/oasis_server/tx_reclaim.json -a unix:/serverdir/node/internal.sock
  ```

- 查看解绑期

  server操作

  ```bash
  # 表示等待336个epoch后，网络自动将解绑余额转入普通账户
  cat /opt/oasis_localhost/genesis.json | \
    python3 -c 'import sys, json; \
    print(json.load(sys.stdin)["staking"]["params"]["debonding_interval"])'
  输出内容信息：
  336
  ```




#### zabbix监控节点

1. 使用zabbix自带模版监控CPU/内存/磁盘/网络状态

2. 自定义监控脚本获取节点状态

```bash
#!/usr/bin/python3
# -*- coding: utf-8 -*-
import requests
import json
import sys
import  socket
import binascii

operation = sys.argv[1]
hostname = socket.gethostname()

scan_url1 = "https://www.oasisscan.com/mainnet/dashboard/network"
scan_url2 = "https://www.oasisscan.com/mainnet/validator/info?address=oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66"
data1 = json.loads((requests.get(scan_url1)).text)
data2 = json.loads((requests.get(scan_url2)).text)


if operation == 'minute':
    # 链块高
    print(hostname + " oasis.node[chain_blocknum] "+ str(data1["data"]["curHeight"]))
    # 验证人状态
    status = str(data2["data"]["status"])
    if status == "false":
        print(hostname + " oasis.node[status] "+ str("0"))
    else:
        print(hostname + " oasis.node[status] "+ str("1"))
    # 验证人活跃状态
    active_status = str(data2["data"]["active"])
    if active_status == "false":
        print(hostname + " oasis.node[active] "+ str("0"))
    else:
        print(hostname + " oasis.node[active] "+ str("1"))
elif operation == 'rank':
    # rank
    print(hostname + " oasis.node[rank] " + str(data2["data"]["rank"]))
elif operation == 'day':
    # delegators
    #print(hostname + " oasis.node[delegators] " + str(data2["totalSize"]))
    print(hostname + " oasis.node[delegators] " + str(data2["data"]["delegators"]))
    # commission
    print(hostname + " oasis.node[commission] " + str(float(data2["data"]["commission"])*100))
    # escrow amount
    print(hostname + " oasis.node[escrow] " + str(data2["data"]["escrow"]))
    # self escrow amount
    print(hostname + " oasis.node[escrow_self] " + str(data2["data"]["escrowAmountStatus"]["self"]))
    # other escrow amount
    print(hostname + " oasis.node[escrow_other] " + str(data2["data"]["escrowAmountStatus"]["other"]))
    # totalShares
    print(hostname + " oasis.node[totalShares] " + str(data2["data"]["totalShares"]))
    # self shares
    print(hostname + " oasis.node[shares_self] " + str(data2["data"]["escrowSharesStatus"]["self"]))
    # other shares
    print(hostname + " oasis.node[shares_other] " + str(data2["data"]["escrowSharesStatus"]["other"]))

```



---



如果您有任何问题，请随时使用我们的社交媒体渠道与我们联系，联系方式如下：

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)



# 关于Terminet

Terminet是一家专业的Staking和生态系统服务提供商。核心团队由经验丰富的开发人员、产品和运营专家以及区块链爱好者组成，在区块链行业拥有超过5年的经验。

Terminet致力于帮助投资者和普通Token持有人从数字资产中获取收益。构建可靠、安全、易用的web3基础设施服务层，降低web3连接门槛和web3应用开发门槛，助力web3发展。