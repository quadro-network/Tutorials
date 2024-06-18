<p style="font-size:14px" align="right">
<a href="https://t.me/quadro_network" target="_blank">Join our telegram <img src="https://user-images.githubusercontent.com/50621007/183283867-56b4d69f-bc6e-4939-b00a-72aa019d1aea.png" width="30"/></a>
<a href="https://discord.com/users/1225408886966714388" target="_blank">Join our discord <img src="https://user-images.githubusercontent.com/50621007/176236430-53b0f4de-41ff-41f7-92a1-4233890a90c8.png" width="30"/></a>
<a href="https://quadro.network/" target="_blank">Visit our website <img src="https://assets.quadro.network/img/icon_min.png" width="30"/></a>
</p>
<p align="center">
  <img height="100" height="auto" src="https://assets.quadro.network/img/1.png">
</p>

# Avail Light Client - Goldberg network

Official documentation:
>- https://github.com/availproject/avail

Explorer:
>- https://telemetry.avail.tools
>- https://kate.avail.tools/

Fill form for Avail Light client: 
>- https://docs.google.com/forms/d/e/1FAIpQLSeL6aXqz6vBbYEgD1cZKaQ4vwbN2o3Rxys-wKTuKySVR-oS8g/viewform

## Recommended Hardware Requirements 
Minimum
>- 4GB RAM
>- 2core CPU (amd64/x86 architecture)
>- 20-40 GB Storage (SSD)

Recommended
>- 8GB RAM
>- 4core CPU (amd64/x86 architecture)
>- 200-300 GB Storage (SSD)

## Set up your Avail Light Node
### Option 1 (automatic)
You can setup your avail light node in few minutes by using automated script below.
```
wget -O avail-light.sh https://raw.githubusercontent.com/thenhthang/Quadro/main/Avail/avail-light.sh && chmod +x avail-light.sh && ./avail-light.sh
```

### Option 2 (manual)
## Update packages
```
sudo apt update && sudo apt upgrade -y
```
## Install dependencies
```
sudo apt install curl tar wget clang pkg-config protobuf-compiler libssl-dev jq build-essential protobuf-compiler bsdmainutils git make ncdu gcc git jq chrony liblz4-tool -y
```
## Install Rust
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
rustup default stable
rustup update
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly
```
## Download and build binaries
```
git clone https://github.com/availproject/avail-light.git
cd avail-light
wget -O config.yaml https://raw.githubusercontent.com/thenhthang/Quadro/main/Avail/config.yaml
git checkout v1.7.4
cargo build --release
cp -r target/release/avail-light /usr/local/bin
```
## Create service
```
sudo tee /etc/systemd/system/availightd.service > /dev/null <<EOF
[Unit]
Description=Avail Light Client
After=network-online.target

[Service]
User=$USER
ExecStart=$(which avail-light) --network goldberg
Restart=on-failure
RestartSec=3
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```
## Register and start service
```
sudo systemctl daemon-reload
sudo systemctl enable availightd
sudo systemctl restart availightd
```
## Logs and Monitor
### View the logs from the running service: 
```
journalctl -f -u availightd.service
```
### Stop node
```
sudo systemctl stop availightd
```
### Fetching the number of the latest block processed by light client
```
curl "http://localhost:7000/v1/latest_block"
```
### Fetching the confidence for given block
```
curl "http://localhost:7000/v1/confidence/1"
```
### Fetching decoded application data for given block
```
curl "http://localhost:7000/v1/appdata/1?decode=true"
```
### Get the status of a latest block
```
curl "localhost:7000/v1/status"
```
### Get the latest block
```
curl "localhost:7000/v1/latest_block"
```
### Health check
```
curl -I "localhost:7000/health"
```
### More
>- https://github.com/availproject/avail-light#configuration-reference
