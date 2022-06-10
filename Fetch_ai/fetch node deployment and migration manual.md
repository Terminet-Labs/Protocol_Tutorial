# Fetch.ai Node Deployment and Migration Tutorial

Fetch.ai-An open-access decentralized machine learning blockchain-based network.

If you are interested in Fetch.ai network and want to run a validator node, this document will hopefully give you some help.

Terminet has been deploying validators in the fetch.ai network for some time. Although there is official validator documentation, but the official documentation is not perfect, lacking the introduction of the overall process, the introduction of detailed operations, and the instructions on migration.

Terminet combines its actual node operation and maintenance experience, and summarizes and refines the overall operation process and information of validator deployment (including stake) and O&M (migration), and organizes this guide tutorial in a concise and clear way for community reference, which can meet the use of beginner O&M personnel.



**Welcome to delegate FET to Terminet**

> Validator:Terminet
>
> Validator Operator Address: fetchvaloper1k43xsdgf528dxp2fn27dq0l9297v4gjqk2heuu
>
> https://explore-fetchhub.fetch.ai/validator/fetchvaloper1k43xsdgf528dxp2fn27dq0l9297v4gjqk2heuu



## Related Links:

> Developer documentation: https://docs.fetch.ai/
>
> Browser: https://explore-fetchhub.fetch.ai/
>
>  https://www.mintscan.io/fetchai



## 1. Node Host Hardware Requirements

| Server                                                | Use              |
| ----------------------------------------------------- | ------------------ |
| Ubuntu18.04、4 CPU、8GB RAM、100GB system disk、300GB data disk | Sync data as new node |

##  2. Node host initialization

1. If there is a data disk, mount it in the /opt directory

/dev/vdb needs to be changed to the actual data disk name

```bash
sudo parted /dev/vdb mklabel gpt
sudo parted  /dev/vdb mkpart primary 0% 100%
sudo mkfs.ext4 /dev/vdb1
echo "/opt /dev/vdb1     ext4    defaults,discard 0 0" |sudo tee -a /etc/fstab
sudo mount -a
```

2. Create a normal user (grant sudo privileges)

Deploy a new node using the fetch user execution

```bash
sudo useradd --system fetch --shell /usr/sbin/login
echo "fetch ALL=(ALL) ALL" |sudo tee -a /etc/sudoers.d/fetch
```

## 3. Deploy a node

### 3.1 Install dependencies

```
sudo apt-get update && sudo apt-get install -y make gcc
wget -O go1.17.3.linux-amd64.tar.gz https://golang.org/dl/go1.17.3.linux-amd64.tar.gz
sudo rm -rf /usr/local/go && sudo  tar -C /usr/local -xzf go1.17.3.linux-amd64.tar.gz && rm go1.17.3.linux-amd64.tar.gz
echo 'export GOROOT=/usr/local/go' >> $HOME/.bashrc
echo 'export GOPATH=/opt/go' >> $HOME/.bashrc
echo 'export GO111MODULE=on' >> $HOME/.bashrc
echo 'export PATH=$PATH:/usr/local/go/bin:/opt/go/bin' >> $HOME/.bashrc && . $HOME/.bashrc
go version
```

### 3.2 Compile the source code

```bash
git clone git@github.com:fetchai/fetchd.git && cd fetchd
git checkout -b v0.10.3
make build
cp ./build/fetchd /usr/bin
fetchd version
# The ./fetchd working directory will be generated in the ~ directory, which can be moved to a custom path and directory name
```

### 3.3 Initialize node

```bash
sudo chown fetch.fetch /opt
mkdir /opt/fetch
fetchd init Terminet --chain-id fetchhub-4 --home /opt/fetch
fetchd config chain-id fetchhub-4 --home /opt/fetch
fetchd config node https://rpc-fetchhub.fetch.ai:443 --home /opt/fetch
# Edit the configuration file app.toml
vim config/app.toml
minimum-gas-prices = "50000afet"

# Download the genesis file
curl https://raw.githubusercontent.com/fetchai/genesis-fetchhub/fetchhub-4/fetchhub-4/data/genesis_migrated_5300200.json --output /opt/fetch
```

### 3.4 Download node snapshot data

```bash
echo "Latest available snapshot timestamp : $(curl -s -I  https://storage.googleapis.com/fetch-ai-mainnet-snapshots/fetchhub-4-pruned.tgz | grep last-modified | cut -f3- -d' ')"

curl -v https://storage.googleapis.com/fetch-ai-mainnet-snapshots/fetchhub-4-pruned.tgz -o- 2>headers.out | tee >(md5sum > md5sum.out) | gunzip -c | tar -xvf - --directory=/opt/fetch

[[ $(grep 'x-goog-hash: md5' headers.out | sed -z 's/^.*md5=\(.*\)/\1/g' | tr -d '\r' | base64 -d | od -An -vtx1 | tr -d ' \n') == $(awk '{ print $1 }' md5sum.out) ]] && echo "OK - md5sum match" || echo "ERROR - md5sum MISMATCH"

echo "Downloaded snapshot timestamp: $(grep last-modified headers.out | cut -f3- -d' ')"
```

### 3.5 Start the fetch node

```shell
nohup fetchd start --p2p.seeds=17693da418c15c95d629994a320e2c4f51a8069b@connect-fetchhub.fetch.ai:36456,a575c681c2861fe945f77cb3aba0357da294f1f2@connect-fetchhub.fetch.ai:36457,d7cda986c9f59ab9e05058a803c3d0300d15d8da@connect-fetchhub.fetch.ai:36458 --home /opt/fetch --moniker <Node Name> 2>&1 >> fetchd.log &
```

### 3.6 Check if block sync is complete

```
# View sync status
curl -s 127.0.0.1:26657/status |  jq '.result.sync_info.catching_up'
# Return false to complete the block synchronization
# View the number of connected nodes
curl -sS -X GET -G -d max=3 -H 'Accept: application/json' "http://localhost:26657/net_info?" | jq -r '.result.n_peers'
```

### 3.7 Create keys
```bash
fetchd keys add <your_key_name> --home /opt/.fetchd
- name: <your_key_name>
  type: local
  address: fetch...........
  pubkey: '{"@type":"/cosmos.crypto.secp256k1.PubKey","key":"..."}'
  mnemonic: ""

```

### 3.8 Create a validator
```bash
# The scan browser currently has 60 valid validators, 1fet=1000000000000000000afet
fetchd tx staking create-validator \
  --amount=<the amount to bond> \
  --pubkey=$(fetchd tendermint show-validator --home /opt/.fetchd/) \
  --moniker="<choose a moniker>" \
  --chain-id=fetchhub-4 \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas-prices 50000afet \
  --from=<key_name>
  
# Query the current pledge amount of validator
fetchd query staking validator <validator operator address>

# Delegation (after being delegated, tokens can only be returned to the original account if they are re-delegated to other validators or unbound; and these two operations take 21 days to complete)
fetchd tx staking delegate < validator operator address> <amount>afet --from <key_name>
```

### 3.9 Edit validator
```bash
fetchd tx staking edit-validator --moniker="<choose a moniker>" --website="https://terminet.io/" --identity="....." --security-contact="<your email address>" --chain-id=fetchhub-4 --commission-rate="0.10" --from=<key_name>
```

## 4. Migrate a node

The migrated new node only needs to complete the above steps to **3.6 Check if block sync is complete** this step (make sure the version of the fetchd binary file is the same as the version of the binary file of the migrated node)

1. After waiting for the new node to complete the synchronization block, stop the fetch process of the original node
2. Package and upload the priv_validator_key.json and priv_validator_state.json files of the original node to the specified path of the new node (After the upload is complete, it is recommended to delete these two files on the original node)

```bash
# Note: Make sure the same priv_validator_key.json does not run on both nodes at the same time, otherwise you will be penalized
scp config/priv_validator_key.json user@ip:/opt/fetch/config/
scp data/priv_validator_state.json  user@ip:/opt/fetch/data/
```

3. Restart the fetch process of the new node
4. At https://explore-fetchhub.fetch.ai/validators, select your own node name and check whether the last 250 blocks are normal



---



If you have any questions, please feel free to reach out to us using our social media channels as below

[*Medium*](https://medium.com/@Terminet) *|* [*Twitter*](https://twitter.com/Terminet123) *|*[*Telegram*](https://t.me/+fQGw5EbQ4TE0NDZl) | [*Discord*](https://discord.gg/t2np2jHVSG)



# About Terminet

Terminet is a professional Staking and ecosystem service provider. The core team consists of experienced developers, product and operations experts and blockchain enthusiasts with more than 5 years of experience in the blockchain industry.

Terminet is dedicated to helping investors and ordinary Token holders earn profits from digital assets. Build a reliable, secure and easy-to-use web3 infrastructure service layer to lower web3 connecting barriers and web3 application development thresholds, powering web3 development.

