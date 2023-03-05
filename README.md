# Service
**Chain ID**: nolus-rila | **Latest Version Tag**: v0.1.43 | **Wasm**: ON

## Dashboard
* [http://grafana.nolus.nolusination.online:9999/d/Nolus-dashbord/nolus-dashbord?orgId=1&refresh=10s](http://grafana.nolus.nolusination.online:9999/d/Nolus-dashbord/nolus-dashbord?orgId=1&refresh=10s)

## Public endpoints
* [https://api.nolus.nolusination.online/](https://api.nolus.nolusination.online/)
* [https://rpc.nolus.nolusination.online/](https://rpc.nolus.nolusination.online/)
* [https://grpc-web.nolus.nolusination.online/](https://grpc-web.nolus.nolusination.online/)



## Installing Node Nolus with Snapshot Instructions

### Update Server
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```
#### Install GO 1.9
```bash
ver="1.19" && \
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz" && \
sudo rm -rf /usr/local/go && \
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz" && \
rm "go$ver.linux-amd64.tar.gz" && \
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile && \
source $HOME/.bash_profile && \
go version
```

##### Install Nolus
```bash
cd $HOME
git clone https://github.com/Nolus-Protocol/nolus-core
cd nolus-core
git checkout v0.1.43
make install
```

###### Create/Recover Wallet
```bash
nolusd keys add <walletname>
nolusd keys add <walletname> --recover
```
####### Download Genesis
```bash
wget -O $HOME/.nolus/config/genesis.json "https://raw.githubusercontent.com/Nolus-Protocol/nolus-networks/main/testnet/nolus-rila/genesis.json"
```
### Set-up Peers & Gas
```bash
sed -i -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0unls\"/" $HOME/.nolus/config/app.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.nolus/config/config.toml
external_address=$(wget -qO- eth0.me) 
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.nolus/config/config.toml
peers="56cee116ac477689df3b4d86cea5e49cfb450dda@54.246.232.38:26656,56f14005119e17ffb4ef3091886e6f7efd375bfd@34.241.107.0:26656,7f26067679b4323496319fda007a279b52387d77@63.35.222.83:26656,7f4a1876560d807bb049b2e0d0aa4c60cc83aa0a@63.32.88.49:26656,3889ba7efc588b6ec6bdef55a7295f3dd559ebd7@3.249.209.26:26656,de7b54f988a5d086656dcb588f079eb7367f6033@34.244.137.169:26656"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.nolus/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.nolus/config/config.toml
sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.nolus/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.nolus/config/config.toml
```

### Download addrbook 
```bash
wget -O $HOME/.nolus/config/addrbook.json "https://raw.githubusercontent.com/obajay/nodes-Guides/main/Nolus/addrbook.json"
```

### SnapShot installation
```bash
cd $HOME
apt install lz4
```
### Stop Service & Reset Data
```bash
sudo systemctl stop nolusd
cd $HOME/.nolus/data/priv_validator_state.json $HOME/.nolus/data/priv_validator_state.json.backup
rm -rf $HOME/.nolus/data
```

### Installing Snapshot
```bash
curl -L http://snapshot.nolus.nolusination.online/nolus/nolus-snapshot-20230304.tar.lz4 | tar -Ilz4 -xf - -C $HOME/.nolus
mv $HOME/.nolus/data/priv_validator_state.json.backup $HOME/.nolus/data/priv_validator_state.json
```

#### Start Service & Check Logs
```
sudo systemctl start nolusd && sudo journalctl -fu nolusd -o cat
```
