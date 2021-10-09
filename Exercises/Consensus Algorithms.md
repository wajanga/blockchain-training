# Module 4: Consensus Algorithms Exercises

## Exercise 1: Set up a PoA based Private Blockchain
1.	Create 2 user accounts
geth –datadir chaindata/poa account new
2.	Create PoA genesis file
puppeth
3.	Initiate the blockchain
geth –datadir chaindata/poa init genesis.json
. Select PoA as the consensus algorithm
. Add signer 
. Prefund the account
4.	Run the blockchain
geth –datadir chaindata/poa –mine console
5.	Check mining
eth.mining
6.	Check balance
eth.getBalance(eth.coinbase)
7.	Unlock signer
personal.unlockAccount(eth.coinbase)
8.	Check mining
eth.mining
9.	Check balance
eth.getBalance(coinbase)
10.	txn-id=eth.sendTransaction({from:coinbase, to:eth.accounts[1], value: 1000000})
11.	Check transaction details
eth.getTransactions(txn-id)
12.	Check balance
eth.getBalance(eth.accounts[1])