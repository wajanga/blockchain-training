# Private Network

## Initialize the private network
    - geth --datadir private_chain_data init genesis_private.json
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
    - miner.stop()
    - eth.blockNumber
    - eth.getBalance(eth.accounts[0])
    - web3.fromWei(eth.getBalance(eth.accounts[0]), "ether")
### Transactions
    - eth.getBlock(0).transactions.length
    - eth.getBlock(7)
    - var txHash = eth.sendTransaction({ from: eth.accounts[0], to: eth.accounts[1], value: 1000)
    - personal.unlockAccount(eth.accounts[0])
    - eth.getTransaction(txHash)
    - txpool.content.pending
    - eth.getTransactionFromBlock(8, 0)
## Interconnect
    - admin.nodeInfo.enode
    - admin.addPeer("<enode addres>")
    - eth.getBlock("latest")




