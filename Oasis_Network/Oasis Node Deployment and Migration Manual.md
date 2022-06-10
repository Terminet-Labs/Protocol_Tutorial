[toc]

Oasis Network is the leading privacy-enabled and scalable layer-1 blockchain network to propel Web3 forward.

Terminet has been deploying validators in Oasis network for some time, although there are official validator documents, but the overall is rather scattered and there are some cases that do not match with the actual operation process or real environment.

Terminet combines its actual node operation and maintenance experience, and summarizes and refines the overall operation process and information of validator deployment (including stake) and O&M (migration and monitoring), and organizes this guide tutorial in a concise and clear way for community reference, which can meet the use of beginner O&M personnel.



**Welcome to delegate ROSE to Terminet**

>Validator: 0% Fee Terminet
>
>Validator Entity Address: oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66
>
>https://www.oasisscan.com/validators/detail/oasis1qqc35xprrs0x595d9s0hrz0p60aey4cm8ya8at66



#### Resource 

Official website：https://zh.oasisprotocol.org/node

official documentation：https://docs.oasis.dev/general/oasis-network/network-parameters

Oasis browser plugin wallet（Mainnet and testnet support）：https://docs.oasis.dev/general/manage-tokens/oasis-wallets/browser-extension

Web wallet(Mainnet and testnet support)：https://wallet.oasisprotocol.org

Browser(Mainnet and testnet support)：https://www.oasisscan.com

Testnet faucet：https://faucet.testnet.oasis.dev

#### Host Hardware Requirements

 [Official detailed reference](https://docs.oasis.dev/general/run-a-node/prerequisites/hardware-recommendations)

| Server                                                       | Use                                                          |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| Ubuntu18.04、1 CPU、2G memory、50GB system disk              | localhost operation（No network required, used to generate files offline and provided to server deployment nodes） |
| Ubuntu18.04、4 CPU、8G memory、100GB system disk、300GB data disk | Server operation                                             |

#### Host initialization

1. If there is a data disk,Please mount it in the /opt directory

/dev/vdb Need to be modified to actually use the disk name.

```bash
sudo parted /dev/vdb mklabel gpt
sudo parted  /dev/vdb mkpart primary 0% 100%
sudo mkfs.ext4 /dev/vdb1
echo "/opt /dev/vdb1     ext4    defaults,discard 0 0" |sudo tee -a /etc/fstab
sudo mount -a
```

2. Create a non-root user

Perform the steps to run the validator node and staking using the oasis user

```bash
sudo useradd --system oasis --shell /usr/sbin/login
echo "oasis ALL=(ALL) ALL" |sudo tee -a /etc/sudoers.d/oasis
```

3. Increase file descriptor

```bash
# Execute the following command as root user.
cat > /etc/security/limits.d/99-oasis-node.conf << EOF
*        soft    nofile    102400
*        hard    nofile    1048576
EOF
# Check the value. If it is 1048576, it will take effect. If it is not necessary to restart the server, it will take effect.
ulimit -n
```

#### Run the validator node

Except that the genesis file, seed node, and oasis binary are different, the other steps to join the main and test networks are the same, if there is an update, please refer to this link：https://docs.oasis.dev/general/oasis-network/network-parameters

To ensure the security of the key file, the localhost operation and the server operation are executed on different servers

##### Localhost operation

Initialize the entity and authenticator, create the keys and other files needed to deploy the server node

1. Create a directory structure

```bash
sudo chown oasis.oasis /opt
mkdir /opt/oasis_localhost/
cd /opt/oasis_localhost/
mkdir -m700 -p {entity,node}
```

2. Download the oasis-node binary

```shell
wget https://github.com/oasisprotocol/oasis-core/releases/download/v22.1.3/oasis_core_22.1.3_linux_amd64.tar.gz
tar -xf oasis_core_22.1.3_linux_amd64.tar.gz
sudo mv oasis_core_22.1.3_linux_amd64/oasis-node /usr/bin/

# Check version
oasis-node --version
```

3. Download the genesis file, choose either the mainnet or the testnet

```bash
# Mainnet
wget https://github.com/oasisprotocol/mainnet-artifacts/releases/download/2022-04-11/genesis.json -P /opt/oasis_localhost
# Testnet
wget https://github.com/oasisprotocol/testnet-artifacts/releases/download/2022-03-03/genesis.json -P /opt/oasis_localhost
```

4. Initialize entities and nodes

```bash
# Set the genesis file environment variable
GENESIS_FILE_PATH=/opt/oasis_localhost/genesis.json

# Initialize entities, file-based operations
cd /opt/oasis_localhost/entity 
oasis-node registry entity init

# Get id, fill in ENTITY_ID
cat entity.json
{
  "v": 2,
  "id": "ChqSTUlQol8IzTse7PrDa+uGIQqRZFycFrGgIbk5YHk="
}

# Initialize node
cd /opt/oasis_localhost/node
ENTITY_ID=ChqSTUlQol8IzTse7PrDa+uGIQqRZFycFrGgIbk5YHk=
# This STATIC_IP is the public IP of your host
STATIC_IP=8.219.49.103
oasis-node registry node init \
  --node.entity_id $ENTITY_ID \
  --node.consensus_address $STATIC_IP:26656 \
  --node.role validator

# Add node to entity
cd /opt/oasis_localhost/entity
# This will update the entity descriptor entity.json and subsequently the entity_genesis.json file containing the signed entity descriptor payload.
oasis-node registry entity update \
  --entity.node.descriptor /opt/oasis_localhost/node/node_genesis.json
 
```

##### Server operation

1. Create a directory structure

```bash
sudo chown oasis.oasis /opt
mkdir -P /opt/oasis_server/log
mkdir -m700 -p /opt/oasis_server/{etc,node,node/entity}
```

2. Copy the entity and node files to the server host,download the genesis.json genesis file

> Upload the files in the /opt/oasis_localhost/node directory to /opt/oasis_server/node, the specific files are as follows
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
> And make sure all these files have 0600 permissions
>
> chmod -R 600 /opt/oasis_server/node/*.pem
>
> Upload /opt/oasis_localhost/entity/entity.json to /opt/oasis_server/node/entity
>
> Upload /opt/oasis_localhost/genesis.json to /opt/oasis_server/etc/

3. Create a config.yml file and add the following parameters as required

   {{ external_address }} Replace with server public IP）

   Testnet setup

   {{ seed_node_address }} Replace with parameters:05EAC99BB37F6DAAD4B13386FF5E087ACBDDC450@34.86.165.6:26656

   Mainnet settings

   {{ seed_node_address }} Replace with parameters: E27F6B7A350B4CC2B48A6CBE94B0A02B0DCB0BF3@35.199.49.168:26656

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

4. Start node

```bash
nohup oasis-node --config /opt/oasis_server/etc/config.yml 2>&1 > /dev/null &
```

5. View node synchronization status

```bash
oasis-node registry entity list -a unix:/opt/oasis_server/node/internal.sock
The input is as follows:
gb8SHLeDc69Elk7OTfqhtVgE2sqxrBCDQI84xKR+Bjg=
LaKJUKndAqH57DVExspTjdRTRNGJOSqArzM0pm8REZw=
DcplOZzEDthLHtPyjbtn9nldfzZ5b9r8EJFnD3JTGMI=
.....omit
```

6. Check if the node has finished synchronizing the block

```bash
# Completing the block sync will result in:'node completed initial syncing' 。 
oasis-node control is-synced -a unix:/opt/oasis_server/node/internal.sock
```

#### Staking

[Official website staking tips](https://docs.oasis.dev/general/manage-tokens/advanced/oasis-cli-tools/common-staking-info)

##### localhost operation

1. View the staking account address of the entity ID

```shell
# View entity ID that is publickey
cat /opt/oasis_localhost/entity/entity.json
Output content information:
{
  "v": 2,
  "id": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
  "nodes": [
    "jzecOLosQppjlkjjkkt4FcdR2TCGpiFLgQLT1j5Gx5Q="
  ]
 }
# Convert entity ID (Base64 encoded) to pledge account address
$ oasis-node stake pubkey2address --public_key KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=
oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4 # Staking address
```

2. Pledge to generate transaction signature command (staking threshold 200 tokens)

```bash
# GENESIS_FILE_PATH is the genesis file on localhost:/opt/oasis_localhost/genesis.json
# ENTITY_DIR_PATH is the path where the entity on localhost is located:/opt/oasis_localho/entity/
# ACCOUNT_ADDRESS is the account pledge address, which is converted from entity ID: oasis1qpgk5ahlvr97rj76l9e75fwedwwhg25zxqep8jj4
# OUTPUT_TX_FILE_PATH is the signature file output path:/opt/oasis_localhost/signed-escrow.tx
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

# Generated file $OUTPUT_TX_FILE_PATH path:signed-escrow.tx
$ cat signed-escrow.tx
{
  "untrusted_raw_value": "pGNmZWWiY2dhcxkI3WZhbW91bnRCB9BkYm9keaJmYW1vdW50RS6Q7dAAZ2FjY291bnRVAFFqdv9gy+HL2vlz6iXZa510KoIwZW5vbmNlAGZtZXRob2Rxc3Rha2luZy5BZGRFc2Nyb3c=",
  "signature": {
    "public_key": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
    "signature": "vHg76Qq7qDV3QtkmNJbzh6S3aHPrNgOoXs0FNNx3scc5oVXYyFhFwlWZfTGUaerseCMli+hk6b4DWbY3FGfwCQ=="
  }
```

3. Generate entity registration transaction signature command

```shell
# GENESIS_FILE_PATH is the genesis file on localhost:/opt/oasis_localhost/genesis.json
# ENTITY_DIR_PATH is the path where the entity on localhost is located:/opt/oasis_localhost/entity/
# OUTPUT_REGISTER_TX_FILE_PATH is the signature file output path:/opt/oasis_localhost/signed-register.tx
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

# Generate the registration signature file signed-register.tx,the following example shows
$ cat signed-register.tx
{
  "untrusted_raw_value": "pGNmZWWiY2dhcxkJnGZhbW91bnRCA+hkYm9keaJpc2lnbmF0dXJlomlzaWduYXR1cmVYQEK7cRpTwqUvozt35i+L7fSaVEcQWgSnehEJSoEI9yZdsSBgE2D0L4x9YFcMC0EWqgOqS8xYu8ENtaJErYu1mA1qcHVibGljX2tleVggKzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcZzdW50cnVzdGVkX3Jhd192YWx1ZVhSo2F2AmJpZFggKzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcZlbm9kZXOBWCCPN5w4uixCmmOWSOOSS3gVx1HZMIamIUuBAtPWPkbHlGVub25jZQFmbWV0aG9kd3JlZ2lzdHJ5LlJlZ2lzdGVyRW50aXR5",
  "signature": {
    "public_key": "KzotvVsgmTBNDsyHdv0CZzQBBi3r9lcKxWuCD0uFQcY=",
    "signature": "Qrrx1vo0cwsL0g7+9oyBDqsO6GMKYZRfxZFEf3I/Iucc6w4gj2g2eCqjTKXn4RYvjzONxmHEQf90eTnSXJO6AQ=="
  }
```

##### Server operation

Copy the generated two signature files to the server directory and submit

>Upload signed-escrow.tx and signed-register.tx to /opt/oasis_server

```shell
# Commit two transactions
oasis-node consensus submit_tx --transaction.file /opt/oasis_server/signed-escrow.tx -a unix:/serverdir/node/internal.sock

oasis-node consensus submit_tx --transaction.file /opt/oasis_server/signed-register.tx -a unix:/serverdir/node/internal.sock
```

#### Migrating validator node operations

> Note: Stop the original node, package the node directory to the new node and restart it; the oasis node will be offline for a short time and will not have other effects. If it is offline for more than an hour, it will exit the active set, and will resume joining the active set after restarting.

1、Install the binaries on the new host

```shell
# The node binary version must be consistent with the original node binary version
wget https://github.com/oasisprotocol/oasis-core/releases/download/v22.1.7/oasis_core_22.1.7_linux_amd64.tar.gz
tar -xf oasis_core_22.1.7_linux_amd64.tar.gz
cd oasis_core_22.1.3_linux_amd64/
sudo mv oasis-node /usr/bin
# Check version
$ oasis-node --version
Software version: 22.1.7
Consensus:
  Consensus protocol version: 6.0.0
Runtime:
  Host protocol version:      5.0.0
  Committee protocol version: 4.0.0
Go toolchain version: 1.17.10
```

2、Stop the original node process and copy the node data to the new node

```shell
# Operation on the original node
tar -czf oasis.tar.gz oasis_server/
scp oasis.tar.gz user@ip:/opt/

# Operation on the new node host
tar -xf oasis.tar.gz

# Modify configuration file parameters (note whether other file paths have changed, such as logs)
vim /opt/oasis_server/etc/config.yml
external_address: tcp://Public IP of the new node host:26656
```

3、Start new node process

```shell
nohup oasis-node --config /opt/oasis_server/etc/config.yml 2>&1 > oasis-node.log &
```

4、Check if the new node is up and running

```shell
# Query the network status of the node, if the response is that the connection is successful, and the non-zero exit code is received, it is not connected to the network.
$ oasis-node registry entity list -a unix:/opt/oasis_server/node/internal.sock
gb8SHLeDc69Elk7OTfqhtVgE2sqxrBCDQI84xKR+Bjg=
LaKJUKndAqH57DVExspTjdRTRNGJOSqArzM0pm8REZw=
DcplOZzEDthLHtPyjbtn9nldfzZ5b9r8EJFnD3JTGMI=
.....omit
# Check if the node synchronization is complete, the synchronization complete will get: 'node completed initial syncing' .
$ oasis-node control is-synced -a unix:/opt/oasis_server/node/internal.sock
$ tail -f oasis-node.log  |grep block_hash
# View Sync Fast High
{"block_hash":"e9cc7941a246c7079180aa6cd083fa0ca65e25e5e6c121761328e918a9ec4603","block_height":8115558,"caller":"mux.go:818","last_retained_version":0,"level":"debug","module":"abci-mux","msg":"Commit","ts":"2022-04-22T08:22:19.767526073Z"}
```



#### Other operations

- View the current block height of the node

  ```bash
  oasis-node control status -a unix:/opt/oasis_server/node/internal.sock
  ```

- View the pledge information of the network

  ```bash
  $ oasis-node stake info -a unix:/opt/oasis_server/node/internal.sock
  # Token Name ROSE
  Token's ticker symbol: ROSE 
  # 
  Token's value base-10 exponent: 9
  # Total circulation 10 billion
  Total supply: 10000000000.0 ROSE
  # Public pool: over 1.3 billion
  Common pool: 1314419522.946253371 ROSE
  Last block fees: 0.0 ROSE
  Governance deposits: 0.0 ROSE
  # Entity's pledge threshold 100ROSE
  Staking threshold (entity): 100.0 ROSE
  # Validator's pledge threshold 100ROSE
  Staking threshold (node-validator): 100.0 ROSE
  # The staking threshold for computing nodes is 100ROSE
  Staking threshold (node-compute): 100.0 ROSE
  # Staking Threshold for Key Manager 100ROSE
  Staking threshold (node-keymanager): 100.0 ROSE
  # The staking threshold calculated at runtime is 50000ROSE
  Staking threshold (runtime-compute): 50000.0 ROSE
  # Staking Threshold for Runtime Key Manager 50000ROSE
  Staking threshold (runtime-keymanager): 50000.0 ROSE
  ```
  
- Get the gRPC exposed by the Oasis node

  ```bash
  oasis-node registry node list -v -a unix:/opt/oasis_server/node/internal.sock |   jq 'select(.roles | contains("consensus-rpc")) | .tls.addresses'
  ```

- View Commission Plan Rules

  ```bash
  cat /opt/oasis_localhost/genesis.json | \
  python3 -c 'import sys, json; \
  rules = json.load(sys.stdin)["staking"]["params"]["commission_schedule_rules"]; \
  print(json.dumps(rules, indent=4))'
  
  Output content information:
  # rate_change_interval: Commission rate can be changed every epoch
  # rate_bound_lead: The commission rate must be submitted at least 336 epochs in advance, which is used to set the value of the parameter start_epoch in modifying the commission rate
  {
      "rate_change_interval": 1,
      "rate_bound_lead": 336,
      "max_rate_steps": 10,
      "max_bound_steps": 10
  }
  ```

- Modify commission rate

  Localhost operation

  ```bash
  # Stake.commission_schedule.bounds: Commission rate range setting, the format is "start_epoch/rate_min_numerator/rate_max_numerator", the rate is numerator/100000, that is, the range is 0% to 25%; start_epoch means the setting range starts from 13738, refer to the browser's current epoch value of 13737 , not less than or equal to the current epoch, and the range does not exceed the current epoch+336
  # Stake.commission_schedule.rates: commission rate, format "start_epoch/rate_numerator", the rate is rate_numerator/100000
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

  Server operation

  ```bash
  oasis-node consensus submit_tx --transaction.file /opt/oasis_server/tx_amend_commission_schedule.json -a unix:/serverdir/node/internal.sock
  ```

- View the minimum order quantity

  Server operation

  ```bash
  # That is, the minimum order quantity is 100 tokens
  cat /opt/oasis_localhost/genesis.json | \
  python3 -c 'import sys, json; \
  print(json.dumps(json.load(sys.stdin)["staking"]["params"]["min_delegation"], indent=4))'
  
  Output content information:
  "100000000000"
  ```

- Recycling delegated/staking tokens

  Server operation

  Example of viewing account pledge entrustment information

  ```bash
  oasis-node stake account info --stake.account.address <Staking account address> -a unix:/opt/oasis_server/node/internal.sock
  Output content information:
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
        Amount: 200.041348044 TEST (200000000000 shares) # Select the number of shares to be reclaimed in brackets according to the staking amount
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

  Localhost operation

  Select the amount of tokens to be recovered to generate a transaction (the value in the execution command is the number of shares shares)

  ```bash
  # stake.shares: Number of stakes selected for recycling
  # transaction.file: output transaction signature file
  oasis-node stake account gen_reclaim_escrow \
    "${TX_FLAGS[@]}" \
    --stake.shares 200000000000 \
    --stake.escrow.account <Account address> \
    --transaction.file tx_reclaim.json \
    --transaction.nonce 3 \
    --transaction.fee.gas 1000 \
    --transaction.fee.amount 2000
  ```

  Server operation

  Submit transaction

  ```bash
  oasis-node consensus submit_tx --transaction.file /opt/oasis_server/tx_reclaim.json -a unix:/serverdir/node/internal.sock
  ```

- Check the release period

  Server operation

  ```bash
  # Indicates that after waiting for 336 epochs, the network will automatically transfer the unbound balance to the ordinary account
  cat /opt/oasis_localhost/genesis.json | \
    python3 -c 'import sys, json; \
    print(json.load(sys.stdin)["staking"]["params"]["debonding_interval"])'
  Output content information:
  336
  ```



#### Zabbix monitoring node

1. Use zabbix's own template to monitor CPU/memory/disk/network status

2. Custom monitoring script to get node status

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
    # Block height
    print(hostname + " oasis.node[chain_blocknum] "+ str(data1["data"]["curHeight"]))
    # Validator status
    status = str(data2["data"]["status"])
    if status == "false":
        print(hostname + " oasis.node[status] "+ str("0"))
    else:
        print(hostname + " oasis.node[status] "+ str("1"))
    # Validator active status
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



If you have any questions, please feel free to reach out to us using our social media channels as below

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)

# About Terminet

Terminet is a professional Staking and ecosystem service provider. The core team consists of experienced developers, product and operations experts and blockchain enthusiasts with more than 5 years of experience in the blockchain industry.

Terminet is dedicated to helping investors and ordinary Token holders earn profits from digital assets. Build a reliable, secure and easy-to-use web3 infrastructure service layer to lower web3 connecting barriers and web3 application development thresholds, powering web3 development.