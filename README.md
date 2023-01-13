# Mars-Hub

```
NODENAME=Mad
```
```
MARS_PORT=43
echo "export NODENAME=$NODENAME" >> $HOME/.bash_profile
if [ ! $WALLET ]; then
	echo "export WALLET=wallet" >> $HOME/.bash_profile
fi
echo "export MARS_CHAIN_ID=ares-1" >> $HOME/.bash_profile
echo "export MARS_PORT=${MARS_PORT}" >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```
sudo apt update && sudo apt upgrade -y
```
```
sudo apt install curl build-essential git wget jq make gcc tmux chrony -y
```
```
cd $HOME
rm -rf hub
git clone https://github.com/mars-protocol/hub.git && cd mars
git checkout v1.0.0-rc7
make install
```
```
marsd version --long
```
```
marsd config chain-id $MARS_CHAIN_ID
marsd config node tcp://localhost:${MARS_PORT}657
marsd config keyring-backend test
```

```
marsd init $NODENAME --chain-id $CHAIN_ID
```
```
curl -o $HOME/.mars/config/genesis.json https://raw.githubusercontent.com/mars-protocol/networks/blob/main/ares-1/genesis.json
```

```
marsd keys add $WALLET --recover
```
```
```
```
sed -i.bak -e "s%^proxy_app = \"tcp://127.0.0.1:26658\"%proxy_app = \"tcp://127.0.0.1:43658\"%; s%^laddr = \"tcp://127.0.0.1:26657\"%laddr = \"tcp://127.0.0.1:43657\"%; s%^pprof_laddr = \"localhost:6060\"%pprof_laddr = \"localhost:4360\"%; s%^laddr = \"tcp://0.0.0.0:26656\"%laddr = \"tcp://0.0.0.0:43656\"%; s%^prometheus_listen_addr = \":26660\"%prometheus_listen_addr = \":43660\"%" $HOME/.marsd/config/config.toml && sed -i.bak -e "s%^address = \"0.0.0.0:9090\"%address = \"0.0.0.0:4390\"%; s%^address = \"0.0.0.0:9091\"%address = \"0.0.0.0:4391\"%; s%^address = \"tcp://0.0.0.0:1317\"%address = \"tcp://0.0.0.0:4317\"%" $HOME/.marsd/config/app.toml && sed -i.bak -e "s%^node = \"tcp://localhost:26657\"%node = \"tcp://localhost:43657\"%" $HOME/.marsd/config/client.toml 
```
```
pruning="nothing"
pruning_keep_recent="100"
pruning_keep_every="0"
pruning_interval="50"
sed -i -e "s/^pruning *=.*/pruning = \"$pruning\"/" $HOME/.marsd/config/app.toml
sed -i -e "s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/" $HOME/.marsd/config/app.toml
sed -i -e "s/^pruning-keep-every *=.*/pruning-keep-every = \"$pruning_keep_every\"/" $HOME/.marsd/config/app.toml
sed -i -e "s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.marsd/config/app.toml
```
```
sed -i -e "s/prometheus = false/prometheus = true/" $HOME/.marsd/config/config.toml
```
```
sed -i.bak -e "s/^minimum-gas-prices *=.*/minimum-gas-prices = \"0.0001umars\"/;" ~/.marsd/config/app.toml
```
```
```
```
sudo tee /etc/systemd/system/marsd.service > /dev/null <<EOF
[Unit]
Description=mars
After=network-online.target

[Service]
User=$USER
ExecStart=$(which marsd) start --home $HOME/.marsd
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```                                                               
```                                                               
sudo systemctl daemon-reload
sudo systemctl enable marsd
sudo systemctl restart marsd && sudo journalctl -u marsd -f -o cat
```
```
source $HOME/.bash_profile
```
```                                                               
marsd status 2>&1 | jq .SyncInfo
```
```  
MARS_WALLET_ADDRESS=$(marsd keys show $WALLET -a)
MARS_VALOPER_ADDRESS=$(marsd keys show $WALLET --bech val -a)
echo 'export MARS_ADDRESS='${MARS_WALLET_ADDRESS} >> $HOME/.bash_profile
echo 'export MARS_VALOPER_ADDRESS='${MARS_VALOPER_ADDRESS} >> $HOME/.bash_profile
source $HOME/.bash_profile
```
```  
marsd keys list
```  
```  
```  
```  
marsd query bank balances $MARS_WALLET_ADDRESS
```  
```  
quicksilverd tx staking create-validator \
  --amount 50000000uqck \
  --from $WALLET \
  --commission-max-change-rate "0.01" \
  --commission-max-rate "0.1" \
  --commission-rate "0.07" \
  --min-self-delegation "1" \
  --pubkey  $(quicksilverd tendermint show-validator) \
  --identity=B08700D1C239CD1A \
  --website="https://github.com/madnoder" \
  --details="Cosmos validator" \
  --moniker $NODENAME \
  --chain-id $QUICKSILVER_CHAIN_ID \
  --security-contact="vova1node@gmail.com" \
  --gas auto
```                  

```
quicksilverd tx staking delegate $QUICKSILVER_VALOPER_ADDRESS 601330uqck --from=$WALLET --chain-id=$QUICKSILVER_CHAIN_ID --gas auto
```
```
quicksilverd tx staking edit-validator \
  --new-moniker="Mad as a hatter" \
  --chain-id=quicksilver-1 \
  --from=quick164kd2jgy4evt0zrl8z8nd3hspp777v8s290hnz
```
```
quicksilverd tx distribution withdraw-rewards quickvaloper164kd2jgy4evt0zrl8z8nd3hspp777v8s5gp6at --from=quick164kd2jgy4evt0zrl8z8nd3hspp777v8s290hnz --commission --chain-id=$QUICKSILVER_CHAIN_ID
```
```
for i in {1..10}; do quicksilverd status --node https://quicksilver-testnet.rpc.kjnodes.com/ | jq .SyncInfo.latest_block_height && quicksilverd status | jq .SyncInfo.latest_block_height; sleep 6; done
```

```
cd $HOME
rm -rf hub
git clone https://github.com/mars-protocol/hub.git && cd mars
git checkout v1.0.0-rc7
make install
```
