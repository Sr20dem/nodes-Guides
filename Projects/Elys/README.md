# Elys Testnet guide

![Logo5](https://user-images.githubusercontent.com/44331529/232235690-19de528b-1c02-4fe3-9cde-3d2864b09f22.png)

[WebSite](https://elys.network) \
[GitHub](https://github.com/elys-network/elys)
=
[EXPLORER](https://explorer.stavr.tech/elys-testnet/staking)
=

- **Minimum hardware requirements**:

| Node Type |CPU | RAM  | Storage  | 
|-----------|----|------|----------|
| Testnet   |   4|  8GB | 150GB    |


# 1) Auto_install script
```python
SOOOON
```

# 2) Manual installation

### Preparing the server
```python
sudo apt update && sudo apt upgrade -y
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

## GO 1.20.5
```python
ver="1.20.5"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile
go version
```

# Build 02.07.23
```python
cd $HOME
git clone https://github.com/elys-network/elys elys
cd elys
git checkout v0.8.0
make install
```

*******🟢UPDATE🟢******* 02.07.23
```python
cd $HOME/elys
git fetch --all
git checkout v0.8.0
make install
elysd version --long | grep -e commit -e version
#commit: 567574b84076abfab7c4c0276760578f9f47f875
#version: v0.8.0
sudo systemctl restart elysd && sudo journalctl -u elysd -f -o cat
```

`elysd version --long`
- version: v0.8.0
- commit: 567574b84076abfab7c4c0276760578f9f47f875

```python
elysd init STAVRguide --chain-id elystestnet-1
elysd config chain-id elystestnet-1
```    

## Create/recover wallet
```python
elysd keys add <walletname>
  OR
elysd keys add <walletname> --recover
```

## Download Genesis
```python
wget -O genesis.json https://snapshots.polkachu.com/testnet-genesis/elys/genesis.json --inet4-only
mv genesis.json ~/.elys/config

```
`sha256sum $HOME/.elys/config/genesis.json`
+ 4f0ee139d9ce25ead4195bf7afa4e08422d3a409e0e1d3e271e7d8b512bff0a9

## Set up the minimum gas price and Peers/Seeds/Filter peers/MaxPeers
```python
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0uelys\"/;" ~/.elys/config/app.toml
sed -i -e "s/^filter_peers *=.*/filter_peers = \"true\"/" $HOME/.elys/config/config.toml
external_address=$(wget -qO- eth0.me) 
sed -i.bak -e "s/^external_address *=.*/external_address = \"$external_address:26656\"/" $HOME/.elys/config/config.toml
peers="cdf9ae8529aa00e6e6703b28f3dcfdd37e07b27c@37.187.154.66:26656,db03e6915cad62b2646ae72566ed19074a7707b6@95.217.144.107:22056,df8a39358aaa5d188f59ead77540bc96cf374f82@65.108.9.50:56656,6872ba9c614be18c56a7fd7af7aa3b8ad87e23c4@24.158.14.210:26656,89c4d6fa66c4e4517742e564cd6ba1532496fd43@65.108.108.52:32656,ae22b82b1dc34fa0b1a64854168692310f562136@198.27.74.140:26656,04b100fc6994c78c860e7707a1b66e0f631aecc2@178.128.241.104:26656,a82ae55cc1d96af39977175624537c17f6a70995@137.184.184.159:21956,8723618f5dff7ac9b57472f90f2e86a2eb194e0a@71.236.119.108:25656,d907ce9285951a2a063789df2f6bd4cc86b33d53@142.132.155.178:16656,00c65e06302fb35a1064d9aa4e528aaf98925aa8@65.108.105.48:22056,0cbf883987ff0c8e72f6c75331b2af01c8074946@51.159.223.41:26656,18e2316f09d3a78ffc5d03da731fddd678279653@85.190.246.191:35656,3f75a8743a5b9242cfbb57652defe540a4c1a5d4@137.184.154.151:26656,af58431c7bf3ce9cfc4f77f5243cc40e37454b50@65.109.154.182:40656,3f6a185495fa19ea4272d5f47753fe0042be7d0b@65.108.9.164:21956,734a87b41a015faf59a7d6266deea190421476c2@95.217.160.243:26656,63914e4b213c3579bdfa7be77f6403553b8cb7d5@78.46.61.117:18656,5c2a752c9b1952dbed075c56c600c3a79b58c395@178.211.139.77:27296,defe650a7bda2a55048832ab2e47f34e565130c9@157.230.245.237:21956,f6480d5563172e7de0b97b666c4d503d7c4daae8@94.130.225.23:26656,f3480371baafae419bfef68a64ace00dd8944bd6@65.109.92.241:10126,d9f2e28e398d42fe7ca8ed322ee168b3e867bc95@65.108.199.222:34656,ab4068efcb0e1401ff1b08f9269fa88151a640c0@154.12.229.78:26656,9e456e22da0930be2761123b7036e386a3247647@57.128.110.141:26656,dc06b3547cf81c40c931a748679ce22161e5ac43@148.113.6.121:19656,f29fe386022c463b3945955efe2b753e3bcad9a9@45.151.122.202:26656,e92be3a72a23a0c944633e63a67d0db1587dd98a@167.71.209.28:21956,0c9b0a1bc1ce796c3d9497c7400977fc5bf01379@66.94.101.52:26656,8d9845f7ef934ade824981b9145a26f00192b575@45.79.24.206:26656,42d3a20613e443087ae5aec1f1e56c0a12cf8455@135.181.60.184:46656,5eb7c44a4b448b9122c8c34fdb50f4f48c8ec714@170.64.160.136:26656,536f604d0aaed29669ed90bd7864fe659bfffc9c@104.152.109.134:38656,4404a413b2f2a6ac20aa0424960972528bbcc9ff@31.220.84.183:27656,eb07af5b4c6c0a208cdd8ca0187fe5da2e2602c2@64.226.103.162:26656,7965a8bcea48990e465a87209fdd6375f7d5f94d@64.226.90.157:26656,e8b4a9303c77d1c96ba2ecca28919619f9fa308e@95.216.102.235:21233,af23cecc6b675b5785905199579de84ba36b0a10@65.109.175.192:53656,a42cc9d7134949ce2fa703c6e341a0bd9cc1984c@65.108.206.74:16656,5e6b0be59463073b41499365b8c25a24ad5a07a5@141.98.112.138:61656,71bd5f272277e707b1bec74f0ca10c7ac8472d92@209.145.60.179:26656,501767323c5223bfe138d916189cb5427f7e3931@104.193.254.42:27656,85f34862d3195daaeb6853369bd0439ed1804e8a@159.89.27.173:21956,7a2ba46b795ee84cd73472039faa4b60e0228f0e@81.0.218.194:27656,d5519e378247dfb61dfe90652d1fe3e2b3005a5b@65.109.68.190:53656,701a382e03978c54f1176145460125516b6a4672@3.144.113.232:26656,fec2dfd0a7e0e174e90755eb60c750f5ccc43b40@199.175.98.115:53656,8cc16cba9ccb2e1a555acb29bf53a9198ecae7ce@209.126.2.211:53656,fc5a323a8c57393e84902e832a75f15bd0b898b2@84.46.242.124:53656,599c1fb13feb1bbd44f30cbb00338db686b6106a@159.223.19.22:26656,3f30f68cb08e4dae5dd76c5ce77e6e1a15084346@212.95.51.215:56656,3174bb06e87392c74ad65a80c42feed816366a84@68.183.210.88:21956,e4b07652c318b08357e5796431982169789ce2c5@159.65.32.10:21956,01aaf7bce61622ab4f2f6cedbc57fa3aa5d3cf3c@167.235.1.101:26676,09bf7359f3d2b8ef05d328d89019204d6627f4a4@94.16.117.238:24656,fed5ba77a69a4e75f44588f794999e9ca0c6b440@45.67.217.22:21956,c21e4205b3ce3a85aac5319515433e591d3e181f@139.59.110.197:26656,1fe8bbd7f3ca77985f77f39a664abfd3744d4e6a@143.198.192.11:26656,d412bdd0e608d07415eab12586ed7418a7821379@38.242.153.15:21956,8aa0021c45a64f736e2192f5e520c768bc9fbae2@134.122.75.207:21956,116521cee5c0a5a48eec263fb21b88d559e89f2c@194.163.167.138:54656,54114ce29b4625d75760851e71921d27bba0032a@157.245.201.247:21956,b904eb8b81f58608a2c4a086860fbd52d00ccba6@65.108.226.25:36656,45e30968d5a122a5d8e8e8c36635e6efec112839@45.151.123.12:21956,9655cfb7db5ff69586359c42db7fb8dbe7555613@167.235.7.34:57656,b311e76cf8f66f52d144e1640471d49845c71ff9@108.175.1.36:21956,0986b43c8562b0ccac19ee7cdcfc649ae2b22190@65.109.116.204:21956,f64d9f82cc0ed53377d362fc648b959f6aa426dd@75.119.154.0:21956,6564e7b61aa54b00768573694f3de160961e48d9@144.91.64.15:21956,4b9789401d7c7833bbc577bae003bcfbd3656bba@65.109.28.226:17656,61284a4d71cd3a33771640b42f40b2afda389a1e@5.101.138.254:26656,15263a87a09f90ba71d35cbddf17ff5178e9b133@65.21.225.10:40656,a98484ac9cb8235bd6a65cdf7648107e3d14dab4@116.202.231.58:53656,1af9a47eae993ea84752fff373ec2c7eb27d5918@145.224.88.118:26656,ee401fbe1afe6546f78c8e0f5ee0ff8922a45b06@192.3.164.17:26656,9aa8a73ea9364aa3cf7806d4dd25b6aed88d8152@190.2.136.144:11856"
sed -i.bak -e "s/^persistent_peers *=.*/persistent_peers = \"$peers\"/" $HOME/.elys/config/config.toml
seeds=""
sed -i.bak -e "s/^seeds =.*/seeds = \"$seeds\"/" $HOME/.elys/config/config.toml
sed -i 's/max_num_inbound_peers =.*/max_num_inbound_peers = 50/g' $HOME/.elys/config/config.toml
sed -i 's/max_num_outbound_peers =.*/max_num_outbound_peers = 50/g' $HOME/.elys/config/config.toml

```
### Pruning (optional)
```python
pruning="custom"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="10"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.elys/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.elys/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.elys/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.elys/config/app.toml
```
### Indexer (optional) 
```bash
indexer="null" && \
sed -i -e "s/^indexer *=.*/indexer = \"$indexer\"/" $HOME/.elys/config/config.toml
```

## Download addrbook
```python
wget -O $HOME/.elys/config/addrbook.json "https://raw.githubusercontent.com/obajay/nodes-Guides/main/Projects/Elys/addrbook.json"
```

# Create a service file
```python
sudo tee /etc/systemd/system/elysd.service > /dev/null <<EOF
[Unit]
Description=elys
After=network-online.target

[Service]
User=$USER
ExecStart=$(which elysd) start
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
# StateSync Elys Testnet
```python
SOOON
```
# SnapShot Testnet (~0.2GB) updated every 5 hours  
```python
SOOON
```

## Start
```python
sudo systemctl daemon-reload
sudo systemctl enable elysd
sudo systemctl restart elysd && sudo journalctl -u elysd -f -o cat
```

### Create validator
```python
elysd tx staking create-validator \
  --amount 1000000uelys \
  --commission-max-change-rate "0.2" \
  --commission-max-rate "0.50" \
  --commission-rate "0.1" \
  --min-self-delegation "1" \
  --pubkey=$(elysd tendermint show-validator) \
  --moniker 'STAVRguide' \
  --website "" \
  --identity "" \
  --details "" \
  --chain-id elystestnet-1 \
  --from STAVR1 -y
```

## Delete node
```python
sudo systemctl stop elysd && \
sudo systemctl disable elysd && \
rm /etc/systemd/system/elysd.service && \
sudo systemctl daemon-reload && \
cd $HOME && \
rm -rf elys && \
rm -rf .elys && \
rm -rf $(which elysd)
```
#
### Sync Info
```python
elysd status 2>&1 | jq .SyncInfo
```
### NodeINfo
```python
elysd status 2>&1 | jq .NodeInfo
```
### Check node logs
```python
sudo journalctl -u elysd -f -o cat
```
### Check Balance
```python
elysd query bank balances elysd...addressjkl1yjgn7z09ua9vms259j
```
