<a id="anchor"></a>

[<img align="right" alt="Personal Website" width="22px" src="https://raw.githubusercontent.com/iconic/open-iconic/master/svg/globe.svg" />][sei-website]
[<img align="right" alt="Sei Discord" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@v3/icons/discord.svg" />][sei-discord]
[<img align="right" alt="Sei Blog" width="22px" src="https://cdn.jsdelivr.net/npm/simple-icons@3.13.0/icons/medium.svg"/>][sei-blog]

[sei-blog]: https://blog.seinetwork.io/
[sei-website]: https://www.seinetwork.io/
[sei-discord]: https://discord.com/invite/Sei

|Sections|Description|
|-----------------------:|------------------------------------------:|
| [Install the basic environment](#go) | Install golang. Command to check the version|
| [Install other necessary environments](#necessary) | Clone repository. Compilation project |
| [Run Node](#run) |  Initialize node. Create configuration files. Check logs & sync status. |
| [Create Validator](#validator) |  Create valdator & wallet, check your balance. |
| [Useful commands](https://github.com/cosmoswalk/Cosmos_tutorials/blob/main/Useful%20Tools/Other%20commands.md) | The other administration commands. |
| [Explorer](https://sei.explorers.guru/) |  Check whether your validator is created successfully |

 <p align="center"><a href="https://www.seinetwork.io/"><img align="right"width="100px"alt="nibiru" src="https://user-images.githubusercontent.com/93165931/203327706-ff400e4e-3130-4a24-9f14-52ca3cb83db4.png"></p</a>

| Minimum configuration                                                                                |
|------------------------------------------------------------------------------------------------------|
- 3.2 GHz x4 CPU (recommended 4.2 GHz x6 CPU)                                                                                                
- 8 GB RAM (The requirements written in the official tutorial are too high, the actual 16GB+ is enough) 
- 100-500 GB NVME SSD                                                                                            

--- 
### -Install the basic environment
#### The system used in this tutorial is Ubuntu20.04, please adjust some commands of other systems by yourself. It is recommended to use a foreign VPS.
<a id="go"></a>
#### Install golang
```
sudo rm -rf /usr/local/go;
curl https://dl.google.com/go/go1.19.2.linux-amd64.tar.gz | sudo tar -C/usr/local -zxvf - ;
cat <<'EOF' >>$HOME/.profile
export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export GO111MODULE=on
export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin
EOF
source $HOME/.profile
```
#### After the installation is complete, run the following command to check the version

```
go version
```
<a id="necessary"></a>
[Up to sections ↑](#anchor)
### -Install other necessary environments

#### Update apt
```
sudo apt update && sudo apt full-upgrade -y
sudo apt list --upgradable
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```

```
git clone https://github.com/sei-protocol/sei-chain
cd sei-chain
git checkout 1.2.2beta-postfix
make install
```
After the installation is complete, you can run `seid version` to check whether the installation is successful.

<a id="run"></a>
### -Run node

#### Initialize node

```
moniker=YOUR_MONIKER_NAME
seid init $moniker --chain-id=atlantic-sub-2
seid config chain-id atlantic-sub-2
```

#### Download the Genesis file

```
seid tendermint unsafe-reset-all --home $HOME/.sei
rm $HOME/.sei/config/genesis.json
wget -O $HOME/.sei/config/genesis.json "https://raw.githubusercontent.com/sei-protocol/testnet/main/atlantic-subchains/atlantic-sub-2/genesis.json"
```

#### Set peer and seed

```
PEERS="f48eedfb31854a822129b7f857b43969f2526bad@185.144.99.19:26656,2f1e8842dec0a60c79d8fedfe420697661c837c8@195.3.221.191:26656,f61d6ace9a30d371fa2d1b8e04ec11b66c967a63@167.235.6.228:26656,070650355f3e51d5f1f514759ec7602b993588f1@185.248.24.16:26656,e528e2d19e1b611894745fc1a5d3e7802e606f31@95.214.52.173:26656,dd23e8a8f019ff8030a1238f7cbf99601293050e@213.239.218.199:26656,34c734f3908654b53045f06c5fd262efaa6c0766@65.109.27.156:26656,72e5106ce49cb794f8af7196a14916bc06a36465@5.161.75.216:26656,7900d390baf8e6d5ce69225917e8fd64927e94f2@154.12.240.133:26656,8acf073665a756fca2df91b647a280ef0d05dc8a@85.114.134.203:26656,263803aef62e933f568ced5df5ca2e24d0f9d329@95.216.40.123:26656,5cb50c4b80dff5a92d232057d07f97ab82895cea@65.108.246.4:26656,0174c55cc5fb6c7ad0c39e709710adfb1ee6bae8@49.12.15.138:26656,26ff7747fd64c703bd241bdad3cf75bbda5ae72b@85.10.199.157:26656,390be417d37cb2ac0ee72a7c40f2ead6aa98e62b@65.108.60.151:26656,5d0cee85dcac7364fb8861201eec3a767873bdf3@172.31.16.93:26656,62ec353a7c234ef436518a7d07eed422064c01c9@172.31.16.93:26656,2743782c2bdc22e51250c5edc21048d1e3a7bf01@172.20.0.75:26656,2743782c2bdc22e51250c5edc21048d1e3a7bf01@172.20.0.75:26656,a5b5ee5888f4a8b66a29184611dd19e4c8ce1c28@5.9.71.9:26656,aaa1da62895d2a8daaf09b235ca82a55c8d9efd7@173.212.203.238:26656,ab082b683c6ecfb1148cb87e0153b036b1ea2283@65.108.199.62:26656,169685c8550d1663ac44a77d8bb03ba681a9582d@45.84.138.127:26656,b2a4e16ef6ec4e2e42ec7c22e530840c16351bfa@135.181.222.185:26656,89ba32810d917a9db78808df338b60abcb7ae3e2@45.94.209.32:26656,e84bbca3bd80c9effba4451dd797a0edb61cb5d2@135.181.143.26:26656,531980d9574d1c619aad8ba9f42703c2c817d9f8@38.242.255.82:26656"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.sei/config/config.toml
seeds=""
  tee $HOME/.sei/data/priv_validator_state.json > /dev/null << EOF
{
  "height": "0",
  "round": 0,
  "step": 0
}
EOF
```
[Up to sections ↑](#anchor)

#### Pruning settings
```
pruning="custom" && \
pruning_keep_recent="100" && \
pruning_keep_every="0" && \
pruning_interval="10" && \
sed -i -e "s|^minimum-gas-prices *=.*|minimum-gas-prices = \"0.0001usei\"|" $HOME/.sei/config/app.toml
sed -i -e "s|^pruning *=.*|pruning = \"custom\"|" $HOME/.sei/config/app.toml
sed -i -e "s|^pruning-keep-recent *=.*|pruning-keep-recent = \"5\"|" $HOME/.sei/config/app.toml
sed -i -e "s|^pruning-keep-every *=.*|pruning-keep-every = \"0\"|" $HOME/.sei/config/app.toml
sed -i -e "s|^pruning-interval *=.*|pruning-interval = \"1000\"|" $HOME/.sei/config/app.toml
```
#### State-sync fast synchronization
```
sudo systemctl stop seid
seid tendermint unsafe-reset-all --home $HOME/.sei
SEEDS=""
PEERS="f48eedfb31854a822129b7f857b43969f2526bad@185.144.99.19:26656,2f1e8842dec0a60c79d8fedfe420697661c837c8@195.3.221.191:26656,f61d6ace9a30d371fa2d1b8e04ec11b66c967a63@167.235.6.228:26656,070650355f3e51d5f1f514759ec7602b993588f1@185.248.24.16:26656,e528e2d19e1b611894745fc1a5d3e7802e606f31@95.214.52.173:26656,dd23e8a8f019ff8030a1238f7cbf99601293050e@213.239.218.199:26656,34c734f3908654b53045f06c5fd262efaa6c0766@65.109.27.156:26656,72e5106ce49cb794f8af7196a14916bc06a36465@5.161.75.216:26656,7900d390baf8e6d5ce69225917e8fd64927e94f2@154.12.240.133:26656,8acf073665a756fca2df91b647a280ef0d05dc8a@85.114.134.203:26656,263803aef62e933f568ced5df5ca2e24d0f9d329@95.216.40.123:26656,5cb50c4b80dff5a92d232057d07f97ab82895cea@65.108.246.4:26656,0174c55cc5fb6c7ad0c39e709710adfb1ee6bae8@49.12.15.138:26656,26ff7747fd64c703bd241bdad3cf75bbda5ae72b@85.10.199.157:26656,390be417d37cb2ac0ee72a7c40f2ead6aa98e62b@65.108.60.151:26656,5d0cee85dcac7364fb8861201eec3a767873bdf3@172.31.16.93:26656,62ec353a7c234ef436518a7d07eed422064c01c9@172.31.16.93:26656,2743782c2bdc22e51250c5edc21048d1e3a7bf01@172.20.0.75:26656,2743782c2bdc22e51250c5edc21048d1e3a7bf01@172.20.0.75:26656,a5b5ee5888f4a8b66a29184611dd19e4c8ce1c28@5.9.71.9:26656,aaa1da62895d2a8daaf09b235ca82a55c8d9efd7@173.212.203.238:26656,ab082b683c6ecfb1148cb87e0153b036b1ea2283@65.108.199.62:26656,169685c8550d1663ac44a77d8bb03ba681a9582d@45.84.138.127:26656,b2a4e16ef6ec4e2e42ec7c22e530840c16351bfa@135.181.222.185:26656,89ba32810d917a9db78808df338b60abcb7ae3e2@45.94.209.32:26656,e84bbca3bd80c9effba4451dd797a0edb61cb5d2@135.181.143.26:26656,531980d9574d1c619aad8ba9f42703c2c817d9f8@38.242.255.82:26656"; \
sed -i.bak -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.sei/config/config.toml
SNAP_RPC="http://185.144.99.19:26657"
LATEST_HEIGHT=$(curl -s $SNAP_RPC/block | jq -r .result.block.header.height); \
BLOCK_HEIGHT=$((LATEST_HEIGHT - 1000)); \
TRUST_HASH=$(curl -s "$SNAP_RPC/block?height=$BLOCK_HEIGHT" | jq -r .result.block_id.hash)

sed -i.bak -E "s|^(enable[[:space:]]+=[[:space:]]+).*$|\1true| ; \
s|^(rpc_servers[[:space:]]+=[[:space:]]+).*$|\1\"$SNAP_RPC,$SNAP_RPC\"| ; \
s|^(trust_height[[:space:]]+=[[:space:]]+).*$|\1$BLOCK_HEIGHT| ; \
s|^(trust_hash[[:space:]]+=[[:space:]]+).*$|\1\"$TRUST_HASH\"| ; \
s|^(seeds[[:space:]]+=[[:space:]]+).*$|\1\"\"|" $HOME/.sei/config/config.toml
```
[Up to sections ↑](#anchor)
#### Start node 
```
sudo tee <<EOF >/dev/null /etc/systemd/system/seid.service
[Unit]
Description=Sei-Network Node
After=network.target

[Service]
Type=simple
User=$USER
ExecStart=$(which seid) start
WorkingDirectory=/root/
Restart=on-failure
StartLimitInterval=0
RestartSec=3
LimitNOFILE=65535
LimitMEMLOCK=209715200

[Install]
WantedBy=multi-user.target
EOF
```
```
sudo systemctl daemon-reload && \
sudo systemctl enable seid && \
sudo systemctl start seid
```
___

#### Show log
```
sudo journalctl -u seid -f
```
#### Check sync status
```
curl -s localhost:26657/status | jq .result | jq .sync_info
```
The display `"catching_up":` shows `false` that it has been synchronized. Synchronization takes a while, maybe half an hour to an hour. If the synchronization has not started, it is usually because there are not enough peers. You can consider adding a Peer or using someone else's addrbook.

[Up to sections ↑](#anchor)
#### Replace addrbook
```
wget -O $HOME/.sei/config/addrbook.json "https://raw.githubusercontent.com/cosmoswalk/Cosmos_tutorials/main/Sei/addrbook.json"
```
<a id="validator"></a>
### Create a validator
#### Create wallet
```
seid keys add WALLET_NAME
```
----
## `Note please save the mnemonic and priv_validator_key.json file! If you don't save it, you won't be able to restore it later.`
----
### Receive test coins
#### Go to sei discord [https://discord.gg/nsV3a5CdC9](https://discord.com/channels/973057323805311026/979272741150687262)
[Up to sections ↑](#anchor)
#### Sent in #faucet channel
```
!faucet WALLET_ADDRESS
```
#### Can be used later
```
seid query bank balances WALLET_ADDRESS
```
#### Query the test currency balance.
#### Create a validator
`After enough test coins are obtained and the node is synchronized, a validator can be created. Only validators whose pledge amount is in the top 100 are active validators.`
```
seid tx staking create-validator \
--from {{KEY_NAME}} \
--chain-id="atlantic-sub-2"  \
--moniker="<VALIDATOR_NAME>" \
--commission-max-change-rate=0.01 \
--commission-max-rate=1.0 \
--commission-rate=0.05 \
--details="<description>" \
--security-contact="<contact_information>" \
--website="<your_website>" \
--pubkey $(seid tendermint show-validator) \
--min-self-delegation="1" \
--amount <token delegation>usei \
--node localhost:26657
```

#### After that, you can go to the block [explorer](https://sei.explorers.guru) to check whether your validator is created successfully.
And [other commands](https://github.com/cosmoswalk/Cosmos_tutorials/blob/main/Useful%20Tools/Other%20commands.md)
----

#### More information 

|[Official website](https://www.seinetwork.io/) |[Official twitter](https://twitter.com/seinetwork) | [Discord](https://discord.com/invite/Sei) | [Github](https://github.com/sei-protocol/sei-chain) | [Documentation](https://docs.seinetwork.io/introduction/overview)|
-----------------------------------------------------------

### [Up to sections ↑](#anchor)
