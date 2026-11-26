OFPAY
Design and Implementation of a Cryptographically Secured Offline Peer-to-Peer Payment System Using Permissioned Blockchain Technology
Project Name: OFPAY
Project Type: Software Engineering Project
Platform: Android-first mobile application
Primary Objective: Enable peer-to-peer payments without an active internet connection, with subsequent backend verification and blockchain reconciliation.
Tagline: Pay anywhere. Reconcile everywhere.


⸻


1. Project Overview
OFPAY is an Android-first offline peer-to-peer payment prototype that enables users to exchange payment information and verify transactions without requiring an active internet connection.
The system uses QR codes to facilitate communication between devices, cryptographic signatures to authenticate transactions, local storage to retain offline transaction records, and a permissioned blockchain network to maintain a tamper-evident record of successfully reconciled transactions.
When internet connectivity becomes available, the application synchronises pending transactions with the backend. The backend validates the transactions before submitting eligible records to the blockchain network.
The prototype will use simulated Ghanaian cedi (GHS) balances. It will not process actual money or integrate with banks or mobile money providers.
2. Problem Statement
Most conventional digital payment systems depend on continuous internet connectivity to authenticate users, verify balances and process transactions.
This creates difficulties in environments where connectivity is unreliable, expensive or unavailable.
OFPAY aims to demonstrate how offline payment exchanges can be implemented through QR-based communication and cryptographic verification, while maintaining transaction integrity through later synchronisation and permissioned blockchain reconciliation.
3. Project Objectives
The project aims to:
Develop an Android application capable of initiating and receiving offline payments.
Implement QR-based payment information exchange between devices.
Use public-key cryptography to authenticate transactions.
Store transaction records securely on devices while offline.
Implement an online synchronisation mechanism for pending transactions.
Use a permissioned blockchain to record successfully reconciled transactions.
Detect duplicate transactions, replay attempts and conflicting spending claims.
Evaluate the performance and limitations of the proposed architecture.
4. Project Scope
Included in the MVP
User registration and authentication.
Simulated digital wallets and balances.
Device-specific cryptographic identities.
Offline spending allowances.
QR-based recipient identification.
QR-based payment exchange.
Local transaction verification.
Offline transaction storage.
Backend synchronisation.
Transaction validation and reconciliation.
Permissioned blockchain integration.
Transaction history and status tracking.
Excluded from the MVP
Actual financial transactions.
Bank and mobile money integrations.
NFC and Bluetooth payment exchange.
Cross-border payments.
Merchant payment processing.
Multi-hop offline payments.
Production-grade financial infrastructure.
5. Proposed Technology Stack
Component Technology
Mobile application Flutter
Primary platform Android
Backend Rust + Axum
Cryptographic engine Rust
Digital signatures Ed25519
Hashing SHA-256
Online database PostgreSQL
Local database SQLite
QR generation/scanning Flutter QR libraries
Blockchain Hyperledger Besu
Consensus QBFT
Smart contracts Solidity
Development containers Podman
CI/CD GitHub Actions
The proposed blockchain architecture should be reviewed before implementation to confirm that the additional complexity is justified by the project’s objectives.
6. System Architecture
OFPAY will consist of five major components.
6.1 Mobile Application
The Flutter application provides the user interface and handles:
Wallet creation and management.
QR generation and scanning.
Payment initiation.
Offline transaction verification.
Local transaction storage.
Synchronisation when connectivity returns.Transaction history and status display.
6.2 Cryptographic Payment Engine
A Rust-based component responsible for:
Generating and managing cryptographic identities.
Signing payment transactions.
Verifying digital signatures.
Validating transaction payloads.
Checking allowance limits.
Detecting replay attempts.
Producing transaction hashes.
6.3 Backend API
A Rust/Axum service responsible for:
Authentication.
Wallet management.
Provisioning offline spending allowances.
Receiving synchronised transactions.
Validating transaction claims.
Detecting conflicting transactions.
Communicating with the blockchain network.
6.4 Database
PostgreSQL will store:
User records.
Wallet information.
Device registrations.
Spending allowances.
Transaction records.
Reconciliation results.
Blockchain transaction references.
SQLite will store locally required wallet metadata, allowances and pending transaction records on mobile devices.
6.5 Permissioned Blockchain
Hyperledger Besu will provide a permissioned blockchain network using QBFT consensus.
A Solidity smart contract will record reconciled transaction identifiers and relevant hashes, prevent duplicate registrations and emit transaction events.
Personal information, private keys and sensitive transaction payloads must not be stored directly on-chain.
7. How OFPAY Works
Stage 1: Wallet Setup
A user registers an account.
The backend creates or registers a wallet.
A device-specific cryptographic key pair is generated.
The public key is registered with the backend.
The user receives an offline spending allowance.
Stage 2: Recipient Identification
The recipient opens the application.
The recipient displays a wallet-identification QR code.
The sender scans the QR code.
The sender’s application identifies the intended recipient.
Stage 3: Offline Payment
The sender enters the payment amount.
The application checks the available offline allowance.
A transaction payload is created.
The transaction is digitally signed.
A payment QR code is generated.
The recipient scans the payment QR code.
The recipient verifies the transaction locally.
The transaction is stored as pending reconciliation.
Stage 4: Synchronisation
When either device regains internet connectivity:
Pending transaction records are submitted to the backend.
The backend validates the signatures and spending allowances.
The backend checks expiry, transaction identifiers and spending conflicts.
Valid transactions proceed to reconciliation.
The blockchain records the reconciled transaction.
The application updates the transaction status.
8. Transaction Lifecycle
The system should maintain explicit transaction states.
State Description
CREATED Transaction has been initiated.
LOCALLY_VERIFIED Recipient has verified the payment locally.
PENDING_SYNC Transaction is awaiting synchronisation.
SERVER_VALIDATION Backend is validating the transaction.
BLOCKCHAIN_SUBMISSION Validated transaction is being submitted to the blockchain.
RECONCILED Transaction has been successfully reconciled.
REJECTED Transaction failed validation.
CONFLICT Conflicting spending claim detected.
EXPIRED Relevant allowance or transaction has expired.
REVIEW_REQUIRED Transaction requires further investigation.
Important: Local verification does not mean that a transaction has been finally settled. The user interface must distinguish pending reconciliation from confirmed reconciliation.
9. Security and Cryptographic Requirements
Security is a central part of OFPAY.
The implementation should consider:
Ed25519 digital signatures.
SHA-256 transaction hashing.
Unique transaction identifiers.
Transaction counters or nonces.
Expiring offline spending allowances.
Device-specific cryptographic identities.
QR payload integrity.
Replay attack prevention.
Duplicate transaction detection.
Secure local key storage.
Server-side validation.
Idempotent synchronisation.
Offline Spending Allowances
An allowance represents a portion of a user’s available funds that has been reserved by the backend for offline spending.
The backend provisions and signs an allowance containing information such as:Wallet identifier.
Device identifier.
Maximum permitted spending amount.
Expiry timestamp.
Allowance identifier.
Unique allowance nonce.
All monetary values must use integer minor units rather than floating-point numbers.
For example:
GH₵30.00 = 3000 pesewas.
Double-Spending Considerations
A major challenge in offline payment systems is double spending.
Digital signatures alone do not prevent a user from copying wallet data, restoring an old device state or attempting to spend the same allowance multiple times.
OFPAY should use reserved offline allowances, device-bound keys where supported, transaction counters and backend conflict detection.
However, a software-only prototype cannot guarantee complete prevention of device cloning or rollback attacks.
The project must clearly demonstrate this limitation rather than claim that offline double spending is impossible.
10. Proposed Functional Requirements
ID Requirement
FR-01 Users must be able to register and authenticate.
FR-02 Users must have a simulated wallet and balance.
FR-03 Users must be able to receive offline spending allowances.
FR-04 Users must be able to identify recipients through QR codes.
FR-05 Users must be able to initiate offline payments.
FR-06 Recipients must be able to verify payment QR codes locally.
FR-07 Pending transactions must be stored locally.
FR-08 Transactions must synchronise when connectivity returns.
FR-09 The backend must validate and reconcile transactions.
FR-10 Reconciled transactions must be recorded on the blockchain.
FR-11 Users must be able to view transaction history and status.
11. Proposed API Endpoints
The backend may expose the following endpoints:
Method Endpoint Purpose
POST /auth/register Register a user
POST /auth/login Authenticate a user
GET /wallets/me Retrieve wallet details
GET /wallets/balance Retrieve simulated balance
POST /allowances Request an offline allowance
GET /allowances/active Retrieve active allowances
POST /transactions/verify Validate a submitted transaction
POST /transactions/sync Synchronise pending transactions
GET /transactions Retrieve transaction history
GET /transactions/{id} Retrieve transaction details
GET /blockchain/transaction/{id} Retrieve reconciliation information
These endpoints are proposed interfaces and should be refined during backend and mobile integration.
12. Database Entities
The proposed database should contain the following core entities:
Users
Wallets
Devices
Device public keys
Offline allowances
Transactions
Blockchain records
Reconciliation events
The team should agree on database schemas and API contracts before parallel implementation begins.
13. Suggested Development Workstreams
These are areas of responsibility to distribute among team members, not fixed individual assignments.
Workstream A: Mobile Application
Flutter project setup.
Wallet interface.
QR generation and scanning.
Offline payment interface.
SQLite integration.
Transaction history.
Synchronisation interface.
Workstream B: Backend and Database
Rust/Axum API.
Authentication.
PostgreSQL schema and migrations.
Wallet management.
Offline allowance provisioning.
Transaction synchronisation.
Backend validation.
Workstream C: Cryptographic Payment Engine
Key generation and management.
Transaction signing.
Signature verification.
Transaction hashing.
Canonical transaction encoding.
Nonce and replay protection.
Allowance validation.
Workstream D: Blockchain Infrastructure
Besu network configuration.
QBFT validator setup.
Solidity smart contract.
Transaction registration.
Duplicate registration prevention.
Backend-to-blockchain integration.
Workstream E: Integration, Testing and Documentation
API contract coordination.
End-to-end integration.
Offline payment testing.
Security testing.
Double-spending simulations.
Performance evaluation.
Technical documentation.
Demonstration preparation.
The team should decide ownership based on members’ experience and availability.
14. Proposed Repository Structure
ofpay/
├── apps/
│   └── mobile/
├── services/
│   └── api/
├── crates/
│   └── payment-engine/
├── blockchain/│   ├── contracts/
│   ├── scripts/
│   └── network/
├── database/
│   ├── migrations/
│   └── seeds/
├── docs/
│   ├── PRD.md
│   ├── architecture.md
│   ├── API.md
│   ├── threat-model.md
│   └── diagrams/
├── tests/
│   ├── integration/
│   ├── security/
│   └── performance/
├── infrastructure/
│   ├── compose.yaml
│   └── ci/
├── .github/
│   └── workflows/
├── .gitignore
├── LICENSE
└── README.md
15. Testing and Evaluation
The project should be evaluated using the following scenarios:
Offline payment: Two devices successfully exchange a payment QR code without internet access.
Transaction synchronisation: A pending transaction is synchronised after connectivity returns.
Invalid signature: A modified payment payload is rejected.
Replay attempt: A previously processed transaction cannot be registered again.
Double-spending simulation: Conflicting claims against the same allowance are detected.
Interrupted synchronisation: Repeated synchronisation does not create duplicate transactions.
Expired allowance: An expired allowance cannot be used to authorise a new payment.
Performance Metrics
Local transaction verification time.
QR payload size.
Offline transaction processing time.
Synchronisation latency.
Blockchain confirmation latency.
Number of transactions processed per second.
Local storage overhead.
16. Proposed Development Timeline
Period Target
Week 1 Requirements, architecture, repository, database and API contracts
Week 2 Mobile foundation, cryptographic engine, QR exchange and local storage
Week 3 Backend synchronisation, Besu infrastructure and smart contract
Week 4 Integration, security testing, performance evaluation and demonstration
The timeline should be adjusted to the actual project deadline and team capacity.
17. Expected Deliverables
At completion, the team should produce:
A functional Android application.
A Rust-based backend API.
A cryptographic payment engine.
A working permissioned blockchain network.
A Solidity smart contract.
PostgreSQL and SQLite database implementations.
Automated tests.
Technical documentation.
Architecture and transaction-flow diagrams.
A working demonstration of offline payment and subsequent reconciliation.
18. Definition of Success
OFPAY will be considered successful when:
Two Android devices can exchange a payment without internet connectivity.
The recipient can locally verify the signed payment.
The transaction is stored locally while offline.
The transaction can be synchronised after connectivity returns.
The backend validates the transaction.
A valid transaction is recorded on the permissioned blockchain.
Duplicate and conflicting transaction claims can be detected.
The team can demonstrate the system’s security model and limitations.
19. Important Architectural Principle
OFPAY does not use blockchain to perform offline consensus.
The mobile devices handle offline payment exchange and local verification. The backend performs authoritative validation when connectivity returns. The permissioned blockchain provides a shared, tamper-evident record of reconciled transactions.
This distinction is essential to the correctness of the architecture.


⸻


Project: OFPAY
Core idea: Cryptographically secured offline payment exchange with online reconciliation through a permissioned blockchain.
Development principle: Modular implementation, clearly defined interfaces, independent testing and continuous integration.
