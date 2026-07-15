# Design and Performance Evaluation of Blockchain-Enabled Policy-Based Access Control (PBAC) for Medical Data Sharing

This repository contains the implementation of a blockchain-enabled Policy-Based Access Control (PBAC) framework integrated with a Dynamic Consent Management System (DCMS) for secure medical data sharing. The framework combines Hyperledger Besu, Solidity smart contracts, InterPlanetary File System (IPFS), and Python-based automation to provide secure, transparent, and patient-centric access control for electronic health records.

The implementation supports the research paper:

Design and Performance Evaluation of Blockchain-Enabled Policy-Based Access Control for Medical Data Sharing

--------------------------------------------------------------------------------

Project Overview

The proposed framework enables secure medical data sharing by combining permissioned blockchain technology with off-chain storage.

Patient medical records are stored securely in IPFS, while only the corresponding IPFS Content Identifier (CID), cryptographic hash, and policy metadata are stored on the blockchain. This hybrid architecture reduces blockchain storage requirements while preserving data integrity through hash verification.

The framework allows patients to define dynamic consent policies specifying:

• Authorized requester
• Access purpose
• Access duration
• Access level (Full or Partial)
• Consent status

Smart contracts automatically evaluate every access request and generate a Granted, Partial Granted, Pending, or Denied decision according to the active policy.

The framework was evaluated using three Hyperledger Besu consensus mechanisms:

• QBFT (Quick Byzantine Fault Tolerance)
• IBFT2 (Istanbul Byzantine Fault Tolerance)
• Clique (Proof of Authority)

--------------------------------------------------------------------------------

Repository Structure

contracts/
    myallcontracts.sol

QBFT/
IBFT2/
Clique/

    docker-compose.yml
    pbac_core.py
    t_cases.py
    d_cases.py
    e_cases.py
    Web3.py
    config/
    quorum-explorer/

benchmarking/

    latency.py
    tps.py
    time.py
    all.py

Each consensus directory contains the complete blockchain configuration together with the Python implementation required to execute the PBAC experiments.

--------------------------------------------------------------------------------

Software Requirements


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


## Tools and Frameworks

- [**Remix IDE**](https://remix.ethereum.org/) – Smart contract development  
- [**Visual Studio Code**](https://code.visualstudio.com/) – Scripting and Web3 API integration  
- [**Web3.py**](https://web3py.readthedocs.io/) – Python blockchain interaction library  
- [**Docker & Besu**](https://besu.hyperledger.org/) – Permissioned blockchain setup  
- [**Quorum Explorer**](https://consensys.net/quorum/tools/) – Transaction and node monitoring  
--------------------------------------------------------------------------------

Implementation Procedure

Step 1. Install Dependencies

sudo apt update

sudo apt install docker.io docker-compose openjdk-21-jdk python3-pip -y

Install the required Python packages.

pip install web3 pandas numpy requests cryptography psutil openpyxl

--------------------------------------------------------------------------------

Step 2. Start the IPFS Service

Start the local IPFS daemon.

ipfs daemon

Verify that the IPFS API is available on:

http://localhost:5001

--------------------------------------------------------------------------------

Step 3. Start the Hyperledger Besu Network

Navigate to the selected consensus directory (QBFT, IBFT2, or Clique) and execute

docker-compose up -d

Verify the containers

docker ps

Check blockchain status

curl -X POST --data '{"jsonrpc":"2.0","method":"eth_blockNumber","params":[],"id":1}' http://localhost:8545

--------------------------------------------------------------------------------

Step 4. Deploy the Smart Contract

Open Remix IDE.

Import

contracts/myallcontracts.sol

Compile using Solidity version 0.8.30.

Connect MetaMask to the local Besu RPC endpoint.

Deploy the contract.

Copy the deployed contract address and ABI.

Update the ABI file and contract address used by pbac_core.py.

--------------------------------------------------------------------------------

Step 5. Configure the Python Framework

Update:

• Contract address

• ABI file

• Dataset location

• Consent file

• IPFS API endpoint

• Besu RPC endpoint

The implementation automatically reads patient records, uploads encrypted or plaintext records to IPFS, registers patient metadata on-chain, creates access-control policies, and evaluates access requests.

--------------------------------------------------------------------------------

Step 6. Execute Experimental Scenarios

The implementation provides three experimental scenarios.

t_cases.py

Normal Policy Lifecycle

Includes consent creation, consent withdrawal, consent restoration, policy revocation, policy expiry, purpose mismatch, access-level mismatch, and replay request testing.

d_cases.py

Delegate Authorization

Evaluates delegated policy creation, delegated consent management, unauthorized delegate requests, and policy revocation.

e_cases.py

Emergency Authorization

Evaluates emergency policy creation, emergency access, incorrect emergency purpose, policy expiry, and emergency policy revocation.

--------------------------------------------------------------------------------

Step 7. Benchmarking

Performance evaluation can be performed using

python latency.py

python tps.py

python time.py

python all.py

The benchmarking framework measures

• Transaction latency

• Workflow throughput (TPS)

• Gas consumption

• CPU utilization

• Memory usage

• IPFS latency

--------------------------------------------------------------------------------

Monitoring

The blockchain network can be monitored using

Quorum Explorer

http://localhost:25000

Grafana

http://localhost:3000

Prometheus

http://localhost:9090

--------------------------------------------------------------------------------

Implementation Workflow

1. Deploy the Besu network.

2. Start the IPFS daemon.

3. Deploy the PBAC smart contract.

4. Configure the contract address and ABI.

5. Upload patient records to IPFS.

6. Store the corresponding CID and cryptographic hash on-chain.

7. Create consent and access-control policies.

8. Execute Normal, Delegate, or Emergency authorization scenarios.

9. Evaluate access requests.

10. Measure system performance using the benchmarking scripts.

--------------------------------------------------------------------------------

License

This repository accompanies the research implementation of the PBAC framework for secure medical data sharing and is intended for academic and research purposes.
