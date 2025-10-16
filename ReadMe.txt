# 🩺 Design and Performance Evaluation of Blockchain Implemented Policy-Based Access Control (PBAC) for Medical Data Sharing

This repository contains the implementation artifacts, smart contracts, configurations, and deployment scripts for the proposed **Policy-Based Access Control (PBAC)** system, integrated with a **Dynamic Consent Management System (DCMS)** using **Hyperledger Besu blockchain**.  
The project supports the research paper:

> **Design and Performance Evaluation of Blockchain Implemented Policy-Based Access Control for Medical Data Sharing**  
> *Author:* Nadeem Yaqub  
> *Year:* 2025  
> *(Under submission to Internet of Things Elsevier)*

---

## 🔍 Overview

The PBAC–DCMS framework enables secure, transparent, and GDPR-compliant access control for electronic health records (EHRs).  
It ensures that patients retain full authority over:
- **Who** accesses their data (role-based),
- **When** and **for how long** access is valid,
- **Why** access is requested (purpose-based),
- and **What** level of data access is granted (granularity).

Smart contracts enforce these rules dynamically, providing **immutability**, **auditability**, and **privacy-by-design** in healthcare environments.

---

## System Model

![PBAC Flow](figs/pbac_flow.png)

The proposed model includes three core entities:
- **Patient** – Registers data, defines policies, and grants consent.  
- **Controller** – Manages policy validation, access verification, and logging.  
- **Requester** – Requests data based on consent and policy rules.  

Each transaction — patient registration, policy creation, access request, and validation — is recorded immutably on the blockchain ledger.

---

## System Architecture

The implementation consists of three interconnected layers:

1. **Entity Layer:** Defines patient, controller, and requester roles.  
2. **Application Layer:** Manages policy registration, consent management, and access control through Solidity-based smart contracts.  
3. **Blockchain Layer:** Hyperledger Besu permissioned network with validators ensuring consensus and immutability.

Consensus protocols tested:
- **QBFT (Quick Byzantine Fault Tolerance)**  
- **IBFT2 (Istanbul Byzantine Fault Tolerance)**  
- **Clique (Proof-of-Authority)**

---

## Prerequisites

Before running the implementation, ensure the following are installed:

| Component | Version | Purpose |
|------------|----------|----------|
| **Ubuntu Server** | 22.04 or 24.04 LTS | Host OS |
| **Docker & Docker Compose** | v27.5.1 / v2.35.1 | Multi-node Besu deployment |
| **OpenJDK** | 17 or 21 | Required for Besu |
| **Hyperledger Besu** | v25.8.0 | Ethereum-compatible blockchain client |
| **Node.js** | v18+ | DApp integration |
| **Python** | v3.12+ | Benchmarking scripts |
| **MetaMask** | v13.3.0 | Wallet connection |
| **Solidity Compiler** | v0.8.30 | Smart contract language |
| **VS Code / Remix IDE** | Latest | Development IDEs |

---

## Tools and Frameworks

- [**Remix IDE**](https://remix.ethereum.org/) – Smart contract development  
- [**Visual Studio Code**](https://code.visualstudio.com/) – Scripting and Web3 API integration  
- [**Web3.py**](https://web3py.readthedocs.io/) – Python blockchain interaction library  
- [**Docker & Besu**](https://besu.hyperledger.org/) – Permissioned blockchain setup  
- [**Quorum Explorer**](https://consensys.net/quorum/tools/) – Transaction and node monitoring  

---

***** Implementation Procedure *****

### Step 1: System Configuration  ###

	We ensured that the server is properly configured with all required dependencies:

By using bash

# Update package lists and install necessary dependencies
	sudo apt update && sudo apt install openjdk-21-jdk python3-pip docker.io docker-compose -y


#Install additional Python packages for Web3 integration
	pip3 install web3

### Step 2: Initialize Besu Network  ###
# Start the Besu network
	docker-compose up -d

# Verified that all services are up
	docker-compose ps
	docker ps -a

# Check the logs to ensure everything is running smoothly
	docker logs -f validator's  # Follow logs for validator's 
	docker logs -f rpc-node    

# Check the Blockchain Status: (Can verify each consensus with different commands)
	curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' http://localhost:8545
	This will return the current block number, confirming if nodes are syncing correctly.

### Step 3: Deploy Smart Contracts ###

# Open Remix IDE:

	Go to Remix IDE, an online IDE (https://remix.ethereum.org/) for Solidity smart contract development.

# Configure Remix IDE to Connect to Besu Network:

	Connect Remix IDE to your Besu network using Injected Web3 via MetaMask. Make sure MetaMask is configured to connect to http://localhost:8545.

# Copy ABI Code & Update JSON File:

	Compile the contract in Remix, copy the ABI code, and paste it into the appropriate .json configuration file for contract interaction in the Python scripts.

# Load Smart Contracts:

	Import the smart contract files from the /contracts/ directory in this repository.

	Select Solidity v0.8.30 as the compiler version.

# Deploy via Injected Provider (MetaMask):

	Ensure MetaMask is connected to your local Besu node via HTTP provider (http://localhost:8545).

	Deploy the contract using Injected Web3 (via MetaMask). Note the deployed contract address.


### Step 4: Test Cases   ###

Test	Policy Type	Consent	Access Level	Purpose	Expected Outcome
1	Normal	True	Full	Treatment	Granted
2	Normal	True	Partial	Checkup	Partial Granted
3	Normal	False	None	Audit	        Denied
4	Normal	False	Full	Lab Results	Pending


###  Step 5: Configure and Deploy the Network in Python Scripts  ###

Update Contract Address:

	Paste the deployed contract address into the Python benchmark script to ensure it interacts with the correct deployed contract.

	Configure Network with Accounts and Ethers:

	Add test accounts in Remix IDE with ETH balances and configure them to be used in the benchmark scripts (test accounts should have Ethers for interaction).

Run the Benchmarking Scripts:

	Execute the benchmarking scripts to evaluate the system's performance:

### Step 6: Run Benchmark Tests ###

Execute benchmarking scripts from the /scripts/ directory:

	python3 benchmark_latency.py
	python3 benchmark_tps.py
	python3 benchmark_time.py
	python3 benchmark_resources.py


### Step 6:Validation and Monitoring  ###

Monitor Node Logs:
	docker logs -f validator's    # Monitor logs for validator's node
	docker logs -f rpc-node      # Monitor logs for RPC node

Monitor Transactions:
	http://localhost:25000/explorer/explorer

