# Module 6: The Ethereum Project
## Activity 1: Smart Contract Deployment Using Geth
1. Reconnect back to your container

        docker ps -a
        docker start node1
        docker exec -it node1 bash

2. Launch Geth

        geth --datadir . --mine console

1. Create contract interface specification
        
        //var bank = eth.contract(ABI);
        var bank = eth.contract(
        [
        {
        "constant": false,
        "inputs": [],
        "name": "spend",
        "outputs": [],
        "payable": false,
        "stateMutability": "nonpayable",
        "type": "function"
        },
        {
        "inputs": [],
        "payable": false,
        "stateMutability": "nonpayable",
        "type": "constructor"
        },
        {
        "payable": true,
        "stateMutability": "payable",
        "type": "fallback"
        }
        ]
        );

        //Test
        bank.abi;

4.	Unlock the launch account
    
        personal.unlockAccount(eth.coinbase,"pass1234",0)

5.	Launch the contract

         var instance = bank.new({data:"0x60806040526004361061001e5760003560e01c806345615bcc14610020575b005b34801561002c57600080fd5b50610035610037565b005b6000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff161461009057600080fd5b670de0b6b3a76400003073ffffffffffffffffffffffffffffffffffffffff163110156100bc57600080fd5b6000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff166108fc3073ffffffffffffffffffffffffffffffffffffffff16319081150290604051600060405180830381858888f1935050505015801561013a573d6000803e3d6000fd5b5056fea265627a7a7231582090e8449f8876c53a9a1c0ea79b65f7326cf802b7cc2dc9805c774098b8879f2b64736f6c634300050b0032", gas: 80000, from: personal.listAccounts[0]});

6.	Check the balance of the contract
7.	Send some ethers (0.5) from account[0] to the contract
8.	Check the balance again
9.	Send some ethers from the contract to account[1]
10.	Send some ethers (3) from account[1] to the contract
11.	Check the balance again
12.	Send some ethers from the contract to account[0]
13.	Send some ethers from the contract to account[1]

## Activity 2: Smart contract development with Remix
### Sample Contracts
      pragma solidity ^0.5.1;
      //1. PiggyBank
      contract Piggybank
      {
          // Piggybank contract that allows
          //spending if savings are > 1 ETH
              address payable public owner; // Contract owner
              constructor() public
              {
                  owner = msg.sender;
              }
          // Creation of contract
              function() payable external
              {
              }
          // Saving funds
              function spend() public
              {
                  require(msg.sender == owner);
                  require(address(this).balance >= 30 ether);
                  owner.transfer(address(this).balance);
              }
      }

      //2. Wallets Lock

      pragma solidity 0.5.15;
      contract Lock {
          // address owner; slot #0
          // address unlockTime; slot #1
          constructor (address owner, uint256 unlockTime) public payable {
              assembly {
                  sstore(0x00, owner)
                  sstore(0x01, unlockTime)
              }
          }

          /**
          * @dev        Withdraw function once timestamp has passed unlock time
          */
          function () external payable {
              assembly {
                  switch gt(timestamp, sload(0x01))
                  case 0 { revert(0, 0) }
                  case 1 {
                      switch call(gas, sload(0x00), balance(address), 0, 0, 0, 0)
                      case 0 { revert(0, 0) }
                  }
              }
          }
      }

      //3. Coin Toss
      pragma solidity ^0.5.1;
      contract Coin
      {
          // Coin toss contract. Allows two bettors to bet on a predefined amount.
          uint256 amount;
          uint256 blockNumber;
          address payable[] bettors;
          // Contract owner
          constructor(uint256 amount_) public
          {
              amount = amount_;
          }
          // Creates the contract.@param amount_ the bet amount, in Wei.
          function bet() payable public
          {
              require(msg.value == amount);
              require(bettors.length < 2);
              blockNumber = block.number + 1;
              bettors.push(msg.sender);
          }
          // Places a bet.
          function toss()public
          {
              require(bettors.length == 2);
              require(blockNumber < block.number);
              uint256 winner = uint256(blockhash(block.number)) % 2;
              bettors[winner].transfer(address(this).balance);
              }
          // Tosses the coin and pays the winner.
      }

1.	Install Remix IDE
2.	Create a new .sol file
3.	Paste the Lock code sample and save the file
4.	Compile the source file
5.	Deploy the byte code to your private blockchain
6.	Interact with the smart contract
