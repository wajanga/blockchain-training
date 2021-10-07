# Private Network

## Initialize the private network
    - geth --datadir private_chain_data init genesis_private.json
### Additional Options
    - --bootnodes enode://c7335e27083a7e4b3eab35b5c3c8b8d1c98516a0a100a9a41c6b46fdaa9398797dc07dab220267c72c710f73ad6ebdf019b5cd6bd526bcb80f72f36f22eedf40@bootnode:30301
    - --ethstats <name>:12345678@<ip-address>:3000
## Start the node
    - geth --console --datadir private_chain_data
## Geth Console
### Status
    - eth.accounts
    - eth.blockNumber
    - eth.getBlock(0)
    - eth.mining
    - net.version
    - net.peerCount
### Accounts
    - personal.newAccount()
    - eth.coinbase
    - eth.getBalance(eth.accounts[0])
### Mining
    - miner.start(1)
    - eth.hashrate
    - miner.stop()
    - eth.blockNumber
    - eth.getBalance(eth.accounts[0])
    - web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
### Transactions
    - eth.getBlock(0).transactions.length
    - eth.getBlock(7)
    - var txHash = eth.sendTransaction({ from: eth.accounts[0], to: eth.accounts[1], value: 1000})
    - personal.unlockAccount(eth.accounts[0])
    - eth.getTransaction(txHash)
    - txpool.content.pending
    - eth.getTransactionFromBlock(8, 0)
## Interconnect
    - admin.nodeInfo.enode
    - admin.addPeer("<enode addres>")
    - eth.getBlock("latest")




