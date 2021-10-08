## Sending Transactions
    - personal.unlockAccount(eth.accounts[0])
    - var txHash = eth.sendTransaction({ from: eth.accounts[0], to: eth.accounts[1], value: 1000})
    - txpool.content.pending

## Block Determination
    - eth.getBlock("latest")
    - eth.blockNumber

## Block Analysis
    - eth.getBlock(8)
    - eth.getBlock(0).transactions.length
    - eth.getTransactionFromBlock(8, 0)

## Finding Transactions
    - eth.getTransaction(txHash)