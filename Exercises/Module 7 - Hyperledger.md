# Module 7: Hyperledger Activities
##  Running a Test Network
### 1. Prerequisites
- [Install](https://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html) Git, cURL, Docker

### 2. Install Fabric and Fabric Samples
The cURL command in the instructions below sets up your environment so that you can run the Fabric test network. Specifically, it performs the following steps:

- Clones the hyperledger/fabric-samples repository.
- Downloads the latest Hyperledger Fabric Docker images and tags them as latest
- Downloads the following platform-specific Hyperledger Fabric CLI tool binaries and config files into the `fabric-samples` `/bin` and `/config` directories. These binaries will help you interact with the test network.
    
        # Create directory
        mkdir -p hyperledger
        cd hyperledger

        # Download the latest release of Fabric samples, docker images, and binaries.
        curl -sSL https://bit.ly/2ysbOFE | bash -s

### 3. Bring up the test network
    # Navigate to the test network directory
    cd fabric-samples/test-network
    
    # Run the following command to remove any containers or artifacts from any previous runs
    ./network.sh down
    
    # Bring up the network by issuing the following command
    # This command creates a Fabric network that consists of two peer nodes, one ordering node
    ./network.sh up

    # Check the containers
    docker ps -a

### 4. Create a channel
Run the following command to create a channel with the default name of mychannel:

    ./network.sh createChannel

### 5. Start a chaincode on the channel
After you have used the network.sh to create a channel, you can start a chaincode on the channel using the following command:

    ./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go

### 6. Interact with the network
#### Add binaries to the path
    # Linux, Mac
    export PATH=${PWD}/../bin:$PATH
    export FABRIC_CFG_PATH=$PWD/../config/

    # Windows
    $Env:PATH="${PWD}\..\bin:$PATH"
    $Env:FABRIC_CFG_PATH="$PWD\..\config\"
    $Env:COMPOSE_CONVERT_WINDOWS_PATHS=1

#### Set the environment variables
    # Linux, Mac
    #Environment variables for Org1
    export CORE_PEER_TLS_ENABLED=true
    export CORE_PEER_LOCALMSPID="Org1MSP"
    export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
    export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
    export CORE_PEER_ADDRESS=localhost:7051

    # Windows
    #Environment variables for Org1
    $Env:CORE_PEER_TLS_ENABLED="true"
    $Env:CORE_PEER_LOCALMSPID="Org1MSP"
    $Env:CORE_PEER_TLS_ROOTCERT_FILE="${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt"
    $Env:CORE_PEER_MSPCONFIGPATH="${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp"
    $Env:CORE_PEER_ADDRESS="localhost:7051"

#### Initialize the ledger with assets
Run the following command to initialize the ledger with assets:

    peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"InitLedger","Args":[]}'
#### Query the ledger
You can now query the ledger from your CLI. Run the following command to get the list of assets that were added to your channel ledger:

    peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'
#### Change the owner
Chaincodes are invoked when a network member wants to transfer or change an asset on the ledger. Use the following command to change the owner of an asset on the ledger by invoking the asset-transfer (basic) chaincode:

    peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'
#### Change variables to organization 2
After we invoke the chaincode, we can use another query to see how the invoke changed the assets on the blockchain ledger. Since we already queried the Org1 peer, we can take this opportunity to query the chaincode running on the Org2 peer. Set the following environment variables to operate as Org2

    # Linux, Mac
    #Environment variables for Org2
    export CORE_PEER_TLS_ENABLED=true
    export CORE_PEER_LOCALMSPID="Org2MSP"
    export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
    export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
    export CORE_PEER_ADDRESS=localhost:9051

    #Windows
    #Environment variables for Org2
    $Env:CORE_PEER_TLS_ENABLED="true"
    $Env:CORE_PEER_LOCALMSPID="Org2MSP"
    $Env:CORE_PEER_TLS_ROOTCERT_FILE="${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt"
    $Env:CORE_PEER_MSPCONFIGPATH="${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp"
    $Env:CORE_PEER_ADDRESS="localhost:9051"
#### Query the chaincode
You can now query the asset-transfer (basic) chaincode running on peer0.org2.example.com:

    peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'
### 7. Bring down the network
When you are finished using the test network, you can bring down the network with the following command:

    ./network.sh down

## Troubleshooting
### Installing Prerequisites
#### 1. Run the following commands in Powershell to convert to WSL 2
    
    wsl.exe --set-version Ubuntu-20.04 2
    wsl.exe --set-default-version 2
#### 2. Install Git and cURL in Ubuntu
    sudo apt update
    sudo apt install git
    sudo apt install curl

#### 3. Install Go in Ubuntu
- Follow instruction from here https://linuxize.com/post/how-to-install-go-on-ubuntu-20-04/