# EX.NO-1: Creating a Private Blockchain

## AIM

To create a Private Blockchain, add nodes, create accounts, transfer Ether into it by creating and deploying a Smart Contract.

---

## PROCEDURE

### 1. Install Geth

- Go to **https://geth.ethereum.org/**
- Download the Windows version.
- During installation, select both **Geth** and **Development Tools**.

### 2. Verify Installation

Run the following command in Command Prompt:

```bash
geth
```

### 3. Create a Private Blockchain Directory

```bash
mkdir go-ethereum
cd go-ethereum
```

### 4. Create Two Nodes

```bash
mkdir node1
mkdir node2
```

### 5. Open VS Code

```bash
code .
```

### 6. Create an Account for Node1

```bash
cd node1
geth --datadir "./data" account new
```

- Save the public address and password of **Node1** in `info.txt`.

### 7. Create an Account for Node2

```bash
cd ..
cd node2
geth --datadir "./data" account new
```

- Save the public address and password of **Node2** in `info.txt`.

---

## Create the Genesis Block

### 8. Create `privateblock.json`

Create a file named `privateblock.json` inside the `go-ethereum` directory.

- Replace **Chain ID** with your own unique Chain ID.
- Verify the Chain ID using **https://chainlist.org/**
- Replace:
  - Initial signer address with **Node1 Address**
  - First node address with **Node1 Address**
  - Second node address with **Node2 Address**
- Set the balance for both nodes as:

```text
3000000000000000000
```

---

## Configure Both Nodes

### 9. Initialize Node1

```bash
cd node1
geth --datadir ./data init ../privateblock.json
```

### 10. Initialize Node2

```bash
cd ../node2
geth --datadir ./data init ../privateblock.json
```

---

## Create Bootnode

### 11. Create Bootnode Directory

```bash
mkdir bnode
cd bnode
```

### 12. Generate Bootnode Key

```bash
bootnode -genkey boot.key
bootnode -nodekey boot.key -verbosity 7 -addr "127.0.0.1:30301"
```

### 13. Save Enode

- Copy the generated **Enode URL** and save it in `info.txt`.

---

## Run the Nodes

### 14. Start Node1

```bash
geth --datadir "./data" \
--port 30304 \
--bootnodes "enode://YOUR_ENODE_VALUE" \
--authrpc.port 8547 \
--ipcdisable \
--allow-insecure-unlock \
--http \
--http.corsdomain="https://remix.ethereum.org" \
--http.api web3,eth,debug,personal,net \
--networkid YOUR_NETWORK_ID \
--unlock YOUR_NODE1_ADDRESS \
--password password.txt \
--mine \
--miner.etherbase YOUR_NODE1_ADDRESS
```

### Start Node2

```bash
geth --datadir "./data" \
--port 30306 \
--bootnodes "enode://YOUR_ENODE_VALUE" \
--authrpc.port 8546 \
--networkid YOUR_NETWORK_ID \
--unlock YOUR_NODE2_ADDRESS \
--password password.txt
```

**Replace:**

- `YOUR_ENODE_VALUE` → Bootnode Enode URL
- `YOUR_NETWORK_ID` → Chain ID
- `YOUR_NODE1_ADDRESS` → Node1 Address
- `YOUR_NODE2_ADDRESS` → Node2 Address

Create a `password.txt` file inside both **node1** and **node2** directories and enter the respective account password.

---

## Deploy Smart Contract

### 15. Open Remix IDE

https://remix.ethereum.org/

### 16. Select Environment

- Deploy & Run Transactions
- Choose **Custom - External HTTP Provider**

### 17. Create Smart Contract

Create a new file named:

```text
New.sol
```

### 18. Deploy Contract

Save the file and click **Deploy**.

### 19. Deployment

The smart contract is deployed successfully on **Node1** and added to the blockchain.

---

# PROGRAM

## Genesis File (`privateblock.json`)

```json
{
  "config": {
    "chainId": 2026,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "berlinBlock": 0,
    "londonBlock": 0,
    "clique": {
      "period": 5,
      "epoch": 30000
    }
  },
  "difficulty": "1",
  "gasLimit": "8000000",
 "extradata": "0x00000000000000000000000000000000000000000000000000000000000000006693affd31d6eb78a50f8aa0f6dbb9112283e2140000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  "alloc": {
    "6693affd31d6eb78a50f8aa0f6dbb9112283e214": {
      "balance": "300000000000000000000"
    },
    "27382cf246f148cb4b826d52312abc05926069e4": {
      "balance": "300000000000000000000"
    }
  }
}
```

---

## Smart Contract (`New.sol`)

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

contract DocumentVerification {

    struct Document {
        uint256 documentId;
        string documentName;
        string extractedData;
        string verifiedData;
        bool verified;
        uint256 timestamp;
    }

    mapping(uint256 => Document) public documents;

    // Store document details on blockchain
    function addDocument(
        uint256 _documentId,
        string memory _documentName,
        string memory _extractedData,
        string memory _verifiedData,
        bool _verified
    ) public {

        documents[_documentId] = Document(
            _documentId,
            _documentName,
            _extractedData,
            _verifiedData,
            _verified,
            block.timestamp
        );
    }

    // Retrieve document details
    function getDocument(uint256 _documentId)
        public
        view
        returns (
            uint256,
            string memory,
            string memory,
            string memory,
            bool,
            uint256
        )
    {
        Document memory d = documents[_documentId];

        return (
            d.documentId,
            d.documentName,
            d.extractedData,
            d.verifiedData,
            d.verified,
            d.timestamp
        );
    }
}
```

---

# OUTPUT

### Deploying Transaction in Remix

<img width="443" height="707" alt="image" src="https://github.com/user-attachments/assets/21a44c3c-482a-4118-b259-174ab926dfb5" />

<img width="376" height="547" alt="image" src="https://github.com/user-attachments/assets/4fbbda5a-e5e2-4010-8f2f-31210150ceb2" />
<img width="382" height="296" alt="image" src="https://github.com/user-attachments/assets/28cafd37-fa6d-4832-b74c-df4d38536ff6" />
<img width="421" height="210" alt="image" src="https://github.com/user-attachments/assets/8127e24e-8dc3-4a5f-866e-7544683e9c98" />

<img width="401" height="551" alt="image" src="https://github.com/user-attachments/assets/4c601a63-3a49-45d8-b2cc-ea33160aad3e" />
<img width="392" height="221" alt="image" src="https://github.com/user-attachments/assets/47864341-f3fc-4115-a025-b7c0ece7bdad" />

### Contract Creation Output in Command Prompt

![Contract Creation Output](https://github.com/user-attachments/assets/fc0eaa31-1340-4c3e-8410-3ca49a1f4dcd)

---

# RESULT

Thus, the Private Blockchain was successfully created. Two nodes were added with individual accounts, Ether was allocated through the genesis block, and a smart contract was successfully deployed on the Ethereum private blockchain.
