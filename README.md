# Geth Tutorial
**Ubuntu**

Update dependencies and systems
```
apt-get install build-essential
sudo apt update && sudo apt upgrade
sudo apt dist-upgrade && sudo apt autoremove
```

### Install Geth
```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt update
sudo apt install geth
```

### Create Directories
```
mkdir tnode1
mkdir vnode1
mkdir vnode2
```

### Generate password file
```
vi password.txt
```

### Generate Keys
```
geth --datadir tnode1/ account new
geth --datadir vnode1/ account new
geth --datadir vnode2/ account new
```

### Install/Build Puppeth
**Install Dependencies**
```
sudo apt install make

cd /tmp
wget https://dl.google.com/go/go1.15.linux-amd64.tar.gz
sudo tar -xvf go1.15.linux-amd64.tar.gz
sudo mv go /usr/local

export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

## enable go without restarting
source ~/.profile

## confirm installation
go version
```
**Build Puppeth**
```
cd ~
git clone https://github.com/ethereum/go-ethereum.git
cd go-ethereum
make all
cd build/bin
ls
```

**Run Puppeth**
```
go-ethereum/build/bin/puppeth
```

**Export Genesis**
(Tutorial link)[https://arctouch.com/blog/how-to-set-up-ethereum-blockchain/]

### Init Nodes
```
cd
geth init xcellerator.json --datadir tnode1/
geth init xcellerator.json --datadir vnode1/
geth init xcellerator.json --datadir vnode2/
```

### Launch tnode1
```
geth --networkid 32958 --datadir tnode1/ --allow-insecure-unlock --rpc --rpcaddr '0.0.0.0' --rpcport 8545 --port 30303 dumpconfig >> tnode1.toml

geth --config tnode1.toml


## get enode info from console
> admin.nodeInfo.enode
```


### Launch vnodes
```
geth --bootnodes <enode here> --networkid 32958 --datadir vnode1/ --port 30313 dumpconfig >> vnode1.toml

geth --bootnodes <enode hlere> --networkid 32958 --datadir vnode2/ --port 30314 dumpconfig >> vnode2.toml

geth --config vnode1.toml --mine --miner.etherbase "0xf2E2885f9bc8E123672dde56049b8DD74fb1760C" --unlock "0xf2E2885f9bc8E123672dde56049b8DD74fb1760C" --password password.txt

geth --config vnode2.toml --mine --miner.etherbase "0x6844Bd2E6c8ef6B46480fb2e997dAa7a81aee991" --unlock "0x6844Bd2E6c8ef6B46480fb2e997dAa7a81aee991" --password password.txt
```

### Demo
```
geth attach
```
