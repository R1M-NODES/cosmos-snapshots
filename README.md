## Quick installation
```bash
bash <(curl -s https://raw.githubusercontent.com/R1M-NODES/osmos-snapshots/master/install.sh)
```

## Run your own backup server with snapshots  
Install requirements  
```bash
sudo apt update && \
sudo apt install curl git docker.io -y
```

Clone github repo  
`git clone https://github.com/R1M-NODES/snapshots.git && cd cosmos-snapshots`  

Create folder for snapshots  
`mkdir $HOME/snapshots/`

Start Nginx via docker  
```bash
cd $HOME; \
docker run --name snapshots \
--restart always \
-v $(pwd)/default.conf:/etc/nginx/conf.d/default.conf \
-v $(pwd)/snapshots/:/root/ \
-p 80:80 \
-d nginx
```

Fill in the variables in the file `agoric_snapshot.sh`  
```
CHAIN_ID="agoric-3"
SNAP_PATH="/root/snapshots/agoric"
LOG_PATH="/root/snapshots/agoric/agoric_log.txt"
DATA_PATH="/root/.agoric/data/"
SERVICE_NAME="agoricd.service"
RPC_ADDRESS="http://localhost:22657"
```
Create new snapshot  
`./agoric_snapshot.sh`  

Check snapshot  
```bash
curl -s http://$(wget -qO- eth0.me)
```

## Automation  
You can add script to the cron  
```cron
# start every day at 00:00
0 0 * * * /bin/bash -c '/root/agoric_snapshot.sh'
```
