🌐 Paradias Supply Chain (PSD)

https://img.shields.io/badge/Solidity-%5E0.8.20-363636?style=for-the-badge&logo=solidity
https://img.shields.io/badge/OpenZeppelin-4.9+-4A8CBF?style=for-the-badge&logo=openzeppelin
https://img.shields.io/badge/License-MIT-green.svg?style=for-the-badge
https://img.shields.io/badge/Tests-Passing-brightgreen?style=for-the-badge
https://img.shields.io/badge/Gas-Optimized-orange?style=for-the-badge


📖 Table of Contents
Overview
Core Features
System Architecture
Smart Contracts
ParadiasToken (ERC‑20)
LinearVesting
Treasury
SimpleDAO
MerkleAirdrop
ParadiasSupplyChain (Orchestrator)
Token Distribution
Security & Audits
Gas Optimization
Prerequisites
Installation & Setup
Using Remix IDE
Using Hardhat
Using Foundry
Testing
Deployment
Generating Merkle Proofs for Airdrop
Contributing
License


🚀 Overview
Paradias Supply Chain (PSD) is a fully‑featured token ecosystem built on Ethereum. It provides:

🔹 A fixed‑supply ERC‑20 token (PSD) with burn capability.
🔹 Linear vesting for gradual token release.
🔹 A treasury with whitelisted withdrawal rights.
🔹 A lightweight DAO for community governance.
🔹 A Merkle‑tree based airdrop – gas‑efficient and secure.



✨ Core Features
Feature	Description
Fixed Supply	200,000 PSD (18 decimals) – no inflation beyond MAX_SUPPLY.
Burnable	Holders can burn their own tokens.
Linear Vesting	Release tokens linearly over a custom duration (e.g., 1 year).
Treasury	Secure vault with whitelist‑based withdrawals; deposits are open.
DAO	Proposal creation, one‑vote‑per‑address (token‑weighted), and execution after voting period.
Merkle Airdrop	Claim tokens using off‑chain generated Merkle proofs – no on‑chain list, low gas.
Ownable	All sensitive functions are restricted to the contract owner.
Reentrancy Guard	All critical functions are protected against reentrancy attacks.
SafeERC20	All token transfers use OpenZeppelin’s safe wrapper.


🧱 System Architecture
The system consists of six contracts:
1. ParadiasToken – the ERC‑20 token itself.
2. LinearVesting – manages locked tokens with linear release.
3. Treasury – holds and distributes funds securely.
4. SimpleDAO – enables on‑chain proposal voting.
5. MerkleAirdrop – distributes tokens to a predefined list via Merkle proofs.
6. ParadiasSupplyChain – a factory that deploys all modules and initialises them with the predefined allocation.


 📄 Smart Contracts
1. ParadiasToken (ERC‑20)
Name: Paradias Supply Chain
Symbol: PSD
Decimals: 18
Max Supply: 200_000 * 10**18
Extends: ERC20Burnable, Ownable
Mint function: Only owner can mint, but never exceeds MAX_SUPPLY.

2. LinearVesting
Locks a fixed amount of tokens and releases them linearly over a set duration.
Key functions:
releasableAmount() – calculates how many tokens are claimable now.
release() – transfers the claimable amount to the beneficiary (non‑reentrant).
State: startTime, duration, totalAmount, releasedAmount.

3. Treasury
A secure vault for storing PSD tokens.
Deposits are open to anyone.
Withdrawals are restricted to addresses on a whitelist (managed by owner).
Uses SafeERC20 and ReentrancyGuard

4. SimpleDAO
A minimal DAO where each address with a positive token balance gets one vote per proposal.
Lifecycle: Proposal creation → voting period → execution (if forVotes > againstVotes).
Only the owner can create and execute proposals (execution can be delegated to a timelock in future upgrades).

5. MerkleAirdrop
Distributes tokens to a list of addresses without storing the list on‑chain.
Claim: Users provide a Merkle proof (generated off‑chain) that proves their inclusion.
Each address can claim only once.
Owner can update the Merkle root.


6. ParadiasSupplyChain (Orchestrator)
Deploys all five modules in a single transaction.
Allocates the 200,000 PSD according to the predefined distribution (see below).
Emits a ContractsDeployed event with all contract addresses.



📊 Token Distribution
Allocation	Percentage	Amount (PSD)
Linear Vesting	50%	100,000
Treasury	30%	60,000
DAO	10%	20,000
Merkle Airdrop	10%	20,000
TOTAL	100%	200,000


🔒 Security & Audits
This project follows industry‑best practices:

OpenZeppelin Libraries – all modules use battle‑tested, audited contracts.
ReentrancyGuard – applied on release(), withdraw(), deposit(), and claim().
SafeERC20 – prevents unexpected failures when transferring tokens.
Merkle Proofs – no on‑chain enumeration, reduces attack surface.
Input Validation – zero‑address checks, amount > 0, timestamp checks.
Ownable – sensitive functions are owner‑only.
Burnable – holders can burn, but total supply can never exceed the cap.



⛽ Gas Optimization
The contracts are carefully optimised to minimise gas costs:

Technique	Applied In
immutable for token, beneficiary, startTime, duration, totalAmount	Vesting, Treasury, Airdrop
No loops – all operations are O(1)	All contracts
calldata instead of memory for external function parameters	All public/external functions
Merkle Tree – stores only 1 bytes32 root instead of a full list	Airdrop
view / pure functions where possible	All getters
Minimal storage writes	All contracts



🧰 Prerequisites
Node.js ≥ 16.x (if using Hardhat/Foundry)
Foundry or Hardhat (for testing and deployment)
Remix IDE (for quick prototyping)
MetaMask or any Ethereum wallet
Infura / Alchemy endpoint (for mainnet/testnet deployment)



🛠 Installation & Setup
Using Remix IDE
Go to Remix IDE.
Create a new file ParadiasSupplyChain.sol and paste the entire contract code.
In the Solidity Compiler tab:
Set compiler version to 0.8.20.
Enable Optimization with 200 runs.
Switch to the Deploy & Run tab.
Choose Injected Provider (MetaMask) or Remix VM.
Deploy the ParadiasSupplyChain contract with the required constructor parameters.



🤝 Contributing
We welcome contributions! Please follow these steps:
Fork the repository.
Create a new branch (git checkout -b feature/your-feature).
Commit your changes (git commit -am 'Add some feature').
Push to the branch (git push origin feature/your-feature).
Open a Pull Request.
Please ensure:
All tests pass.
New features include tests.
Code follows Solidity style guidelines (e.g., NatSpec comments).


📄 License
This project is licensed under the MIT License – see the LICENSE file for details.

🙏 Acknowledgements
OpenZeppelin – for secure and standard smart contract libraries.
MerkleTreeJS – for off‑chain Merkle proof generation.
Foundry & Hardhat – for development and testing frameworks.


📬 Contact & Support
For questions, bug reports, or feature requests, please open an issue or reach out to the maintainers.
