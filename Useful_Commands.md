# Docker Commands
## Starting containers
- docker run --net private-blockchain-net -it --name bootnode eth-node
- docker run --net private-blockchain-net -it -p 8545:8545 --name node1 eth-node
- docker run --net private-blockchain-net -it --name node2 eth-node
- docker run -d -p 3000:3000 -e WS_SECRET=<YOUR_SECRET> kamael/eth-netstats
- docker run -d -p 8080:80 -e APP_NODE_URL=http://localhost:8545 --name eth-explorer alethio/ethereum-lite-explorer
- docker run -it -p 8545:8545 -p 30303:30303 -p 30303:30303/udp --name node1 eth-node

## Create and Connect to a netwpork
- docker network create private-blockchain-net
- docker network connect private-blockchain-net <container-name>

# Geth Commands
## Initialize nodes
- geth --datadir chain_data --http --http.addr "0.0.0.0" --http.corsdomain '*' --bootnodes enode://07ca98251825a0ca1cc43ca13e97cfcaebf4e8eb3c40903810f521c7b819d16df2b1178953dfd6f4d6f30e646d99cb87027716348c1fca1e09b61888fb777d94@host.docker.internal:30301 console
- geth --datadir chain_data --http --http.addr "0.0.0.0" --http.corsdomain '*' --bootnodes enode://c7335e27083a7e4b3eab35b5c3c8b8d1c98516a0a100a9a41c6b46fdaa9398797dc07dab220267c72c710f73ad6ebdf019b5cd6bd526bcb80f72f36f22eedf40@bootnode:30301 --allow-insecure-unlock console
- Options
  - --http.api personal,eth,net,web3
  - --nodiscover
  - --ethstats node_pow:12345678@host.docker.internal:3000
  - --port 30304
  - --http.port 8546

## Create new account
- geth account new --datadir chain_data --password /tmp/password
## Accounts and Balances
- personal.listAccounts
- eth.accounts
- eth.getBalance(personal.listAccounts[0])
- web3.eth.getBalance(personal.listAccounts[1])
- web3.fromWei(web3.eth.getBalance(personal.listAccounts[1]), "Ether")

## Transactions
- web3.personal.unlockAccount(web3.personal.listAccounts[0], null, 60)
- web3.eth.sendTransaction({from:personal.listAccounts[0] , to: personal.listAccounts[1], value:1000 })
- web3.eth.sendTransaction({from:personal.listAccounts[0] , to: "0x056e220F8EFaB611Df5d8d395eC84eb145234F58", value:1000 })
- txpool.content

## Peers
- admin.nodeInfo
- admin.peers
- net.peerCount
- admin.addPeer("enode://549468e6d00e135128af33e03a6d27b0ee5fda7fbd0154b2e83fe68afdfda869eb6ace6ccaefe84ed7a5b804529dcef49f0b5d64be97da87b1e28ddecfca227a@<address>:30303")

## Mining
- miner.start()
- miner.stop()
- eth.mining

## Blocks
- eth.getBlock(3)
- eth.blockNumber

# Bootnode Commands
- bootnode -genkey bootnode.key
- bootnode -nodekey bootnode.key