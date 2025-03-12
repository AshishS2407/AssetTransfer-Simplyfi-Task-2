# Asset Management

## Introduction
This project implements an **Asset Management Chaincode** for Hyperledger Fabric. The chaincode facilitates managing asset life cycles, including operations such as creating, reading, updating, and deleting assets. Additionally, users can query assets based on ownership.

## Features
- **Create Asset:** Adds a new asset to the ledger with an ID, name, owner, and value.
- **Read Asset:** Retrieves asset details by its unique ID.
- **Update Asset:** Modifies an asset's details, such as ownership or value.
- **Delete Asset:** Removes an asset from the ledger.
- **Query Assets by Owner:** Lists all assets owned by a specific user.

---

## Prerequisites
Ensure the following dependencies are installed before setting up the network:

### 1. cURL
Install cURL:
```sh
sudo apt install curl -y
```
Verify installation:
```sh
curl -V
```

### 2. JQ (JSON processor)
Install JQ:
```sh
sudo apt install jq -y
```
Verify installation:
```sh
jq --version
```

### 3. Build Essential
Install Build Essential:
```sh
sudo apt install build-essential
```
Verify installation:
```sh
dpkg -1 | grep build-essential
```

## Setting Up the Hyperledger Fabric Test Network

### 1. Download Fabric
Execute the following commands in your terminal:
```sh
curl -sSLO https://raw.githubusercontent.com/hyperledger/fabric/main/scripts/install-fabric.sh 
```
```sh
chmod +x install-fabric.sh
```

Install Fabric and required dependencies:
```sh
./install-fabric.sh -f '2.5.4' -c '1.5.7'
```

Copy Fabric binaries to the system path:
```sh
sudo cp fabric-samples/bin/* /usr/local/bin
```

---

### 2. Start the Test Network
Navigate to the Fabric samples directory and start the network:
```sh
cd fabric-samples/test-network
./network.sh down
./network.sh up createChannel -ca
```
---

### 3. Deploy the Chaincode
```sh
./network.sh deployCC -ccn basic -ccp ../path-to-chaincode -ccl go
```
Replace `../path-to-chaincode` with the exact path to your chaincode.
You can also change the chaincode name (`basic`) to a name of your choice.

---

### 4. Interacting with Chaincode
#### Set up Environment Variables:
```sh
export FABRIC_CFG_PATH=$PWD/../config/

export CORE_PEER_TLS_ENABLED=true

export CORE_PEER_LOCALMSPID="Org1MSP"

export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt

export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp

export CORE_PEER_ADDRESS=localhost:7051

```

#### Create an Asset:
```sh
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"CreateAsset","Args":["asset1","Laptop","Ashish","3000"]}'
```

#### Read an Asset:
```sh
peer chaincode query -C mychannel -n basic -c '{"function":"ReadAsset","Args":["asset1"]}'
```

#### Update an Asset:
```sh
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"UpdateAsset","Args":["asset1","Gaming Laptop","Ashish","3500"]}'
```

#### Delete an Asset:
```sh
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"Args":["DeleteAsset", "asset1"]}'
```

#### Query Assets by Owner:
```sh
peer chaincode query -C mychannel -n basic -c '{"function":"QueryAssetsByOwner","Args":["Ashish"]}'

```
---
## 🤝 Contributing

We welcome contribution! 🙌 Feel free to fork this project, open issues, or submit pull requests. Let’s build something amazing together! 🚀

---

## 📝 License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more details.

![MIT License](https://img.shields.io/badge/License-MIT-blue.svg)

---

## Conclusion
This chaincode enables secure and transparent asset management in a blockchain network. Follow the setup steps carefully to deploy and test the functionality on Hyperledger Fabric.

For more details, refer to the official Hyperledger Fabric documentation: [Hyperledger Fabric Docs](https://hyperledger-fabric.readthedocs.io/en/latest/index.html).

