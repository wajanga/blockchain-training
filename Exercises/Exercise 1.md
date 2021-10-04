# How to setup your own private Blockchain

## Environment Setup
    - Install latest version of Docker from www.docker.com
    - docker run -it --name node01 ubuntu

## Build your own Blockchain

### 1. Make a new directory
    - cd
    - mkdir -p /home/node/geth
    - cd /home/node/geth
### 2. Tool installation
    - apt-get update
    - apt-get install curl
    - curl -o geth.tar.gz https://gethstore.blob.core.windows.net/builds/geth-alltools-linux-amd64-1.10.8-26675454.tar.gz
    - tar -C geth --strip-components 1 -xzf geth.tar.gz
    - cp geth/* /usr/bin/
    - rm -r geth geth.tar.gz
### 3. Acccount creation
    - geth account new --datadir chain_data
### 4. Set up genesis
    - puppeth
### 5. Create network
    - geth init genesis.json --datadir chain_data
### 6. Start network
    - geth --datadir chain_data --mine --miner.threads 1
    - geth --verbosity 2 console --datadir chain_data --mine --miner.threads 1 --nousb





