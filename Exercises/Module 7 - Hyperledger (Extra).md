# Activity
## 1. Set up a Hyperledger Fabric network with Chainstack
1. Create a consortium project with Chainstack. See [Create a project](https://docs.chainstack.com/platform/create-a-project).
2. Deploy a Hyperledger Fabric network. See [Deploy a consortium network](https://docs.chainstack.com/platform/deploy-a-consortium-network).
## 2. Set up the environment
### Clone repository and install the Hyperledger Fabric v2 binaries
    git clone https://github.com/chainstack/freedom-dividend-chaincode
    
    # macOS
    bash downloadPeerBinary.sh

    # Linux
    bash downloadPeerBinary.sh linux
### Setup the environment variables
In the webapp/server/.env file, replace the ORDERER_NAME, PEER_NAME, and MSP_ID variables with:

- ORDERER_NAME — the orderer name of the network you deployed your peer in. To access the orderer name in Chainstack, from Hyperledger Fabric network, select the Service nodes tab and click on Orderer to access its details page. Here you can copy the Orderer name value.
- PEER_NAME — the name of the peer that you deployed. To access the peer name in Chainstack, from Hyperledger Fabric network, select the Peer nodes tab, click on your peer name to access its details page. Here you can copy the Peer name value.
- MSP_ID — the Membership Service Provider identity (MSP ID). To access the MSP ID in Chainstack, from the Hyperledger Fabric network, click the Details link to open the network details modal. Here you can copy the MSP ID value.

### Export the required files from Chainstack
In Chainstack, export the following files.

#### Network connection profile
1. Navigate to your Hyperledger Fabric network.
2. Click Details.
3. Click Export connection profile.
4. Move the exported file to the webapp/certs/ directory.
#### Orderer TLS certificate
1. Navigate to the Hyperledger Fabric Service nodes tab from the network.
2. Access Orderer.
3. Click Export
4. Unzip the downloaded folder.
5. Move the -cert.pem file to the webapp/certs/ directory.
#### Organization identity ZIP folder
1. Navigate to your Hyperledger Fabric network.
2. Click Details.
3. Access Admin identity.
4. Click Export.
5. Unzip the downloaded folder.
6. Move the msp subdirectory to the webapp/certs/ directory.

## 3. Set up the back end application
    # Install the required dependencies
    cd webapp/server
    npm i

    # Start the back end server
    npm run start
## 4. Set up the front end client
    # Install the required dependencies
    cd webapp/client
    npm i

    # Compile and build the front end client
    npm run build
## 5. Install the chaincode
In the web app at http://localhost:4000, click Install chaincode.
## 6. Approve the chaincode
In the web app at http://localhost:4000, click Approve.

Note that we are using one organization in this example. If there are multiple parties involved and the endorsement policy requires more than one endorser, the chaincode package will have to be approved by others before it can be committed to the network.

The value of the organization approval will change from false to true:

## 7. Commit the chaincode
In the web app at http://localhost:4000, click Commit.

You will see the details of the committed chaincode:

## 8. Interact with the installed chaincode
You can now interact with the installed and committed chaincode.

To read more about the chaincode and the arguments, see the chaincode tutorial at Chainstack Docs.

In the web app at http://localhost:4000, click Access chaincode.

## Troubleshooting
### 1. Missing node-sass
    npm install sass
### 2. Client build error
Change the order of imports in /views/Chaincode.js

    import axios from 'axios';
    import ChaincodeTransactions from '@/components/ChaincodeTransactions.vue';

### 3. Additional Tutorials
https://dzone.com/articles/deploy-a-hyperledger-fabric-v2-web-app-using-the-n