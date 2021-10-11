# Module 7: Hyperledger Activities
##  Running a Test Network
### 1. Prerequisites
- [Install](https://hyperledger-fabric.readthedocs.io/en/latest/prereqs.html) Git, cURL, Docker

### 2. Install Fabric and Fabric Samples
    mkdir -p hyperledger
    cd hyperledger
    curl -sSL https://bit.ly/2ysbOFE | bash -s

### 3. Bring up the test network
    cd fabric-samples/test-network
    ./network.sh down
    ./network.sh up
    docker ps -a

### 4. Create a channel
    ./network.sh createChannel

### 5. Start a chaincode on the channel
    ./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go

### 6. Interact with the network
#### Add binaries to the path
    # Linux, Mac
    export PATH=${PWD}/../bin:$PATH
    export FABRIC_CFG_PATH=$PWD/../config/

    # Windows
    $Env:FABRIC_CFG_PATH="1.3.0"
    $Env:PATH="${PWD}\..\bin:$PATH"
    $Env:FABRIC_CFG_PATH="$PWD\..\config\"

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
    peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"InitLedger","Args":[]}'
#### Query the ledger
    peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'
#### Change the owner
    peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'
#### Change variables to organization 2
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
    peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'
### 7. Bring down the network
    ./network.sh down