# UPI MeshPay

> A secure offline-first UPI payment system using simulated device-to-device mesh communication.

---

## 🚀 Overview

UPI MeshPay is a Spring Boot-based offline payment platform designed
to enable digital transactions in environments where continuous
internet connectivity is unavailable.

### Core Flow

```text
Sender Device
      │
      ▼
Create Payment
      │
      ▼
Encrypted Payment Packet
      │
      ▼
 ┌───────────────┐
 │   Mesh Node   │
 └───────┬───────┘
         │
         ▼
 ┌───────────────┐
 │   Mesh Node   │
 └───────┬───────┘
         │
         ▼
 ┌───────────────┐
 │ Bridge Device │
 └───────┬───────┘
         │
         │ Internet Available
         ▼
 ┌────────────────────┐
 │   Spring Boot API  │
 └─────────┬──────────┘
           │
           ▼
 ┌────────────────────┐
 │ Transaction Engine │
 └─────────┬──────────┘
           │
           ▼
     Settlement

🎯 Problem Statement
Traditional digital payment systems depend heavily on network connectivity.
In remote locations, underground environments, during network outages, or in areas with weak connectivity, users may not be able to initiate or complete digital transactions.
This project explores an offline-first payment architecture where transaction information can travel between nearby devices before reaching an internet-connected bridge.
✨ Key Features
- 🔌 Offline payment creation
- 📡 Simulated device-to-device mesh communication
- 🔐 Secure payment packet encryption
- 🛡️ Transaction integrity verification
- ⏱️ Transaction freshness validation
- 🔄 Idempotent transaction processing
- 🚫 Duplicate transaction prevention
- ⚡ Concurrent transaction handling
- 🌐 Bridge-based backend synchronization
- 💳 Sender and receiver balance management
- 📒 Transaction ledger
- 🗄️ H2 database
- 🔗 RESTful APIs
- 🧪 Automated testing
🏗️ System Architecture
                         OFFLINE ENVIRONMENT

┌──────────────┐
│ Sender Device│
└──────┬───────┘
       │
       │ Create Transaction
       ▼
┌──────────────────┐
│ Payment Packet   │
│ Encryption       │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Mesh Node 1   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Mesh Node 2   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│    Mesh Node 3   │
└────────┬─────────┘
         │
         ▼
┌──────────────────┐
│  Bridge Device   │
└────────┬─────────┘
         │
         │ Internet Available
         ▼

                    ONLINE ENVIRONMENT

┌──────────────────────────┐
│     Spring Boot API      │
├──────────────────────────┤
│ Transaction Validation   │
│ Idempotency Checking     │
│ Security Verification   │
│ Settlement Processing    │
└────────────┬─────────────┘
             │
             ▼
┌──────────────────────────┐
│       H2 Database        │
├──────────────────────────┤
│ Accounts                 │
│ Transactions             │
│ Payment Records          │
└──────────────────────────┘

🔄 Transaction Flow
1. Create Payment
The sender initiates a payment while offline.
Sender → Receiver → Amount → PIN

A payment packet is generated for further propagation.
2. Secure the Payment Packet
The transaction data is protected before being propagated through the mesh.
Transaction
     │
     ▼
Encryption
     │
     ▼
Secure Payment Packet

3. Mesh Propagation
The payment packet travels through simulated nearby devices.
Device A
   ↓
Device B
   ↓
Device C
   ↓
Bridge Device

4. Bridge Synchronization
When the bridge device obtains internet connectivity, it uploads the pending transaction to the backend.
5. Backend Validation
The backend validates the received payment packet before processing it.
6. Transaction Settlement
After successful validation:
Sender Balance
      ↓
    Debit
      ↓
Transaction
      ↓
   Credit
      ↓
Receiver Balance

🔐 Security
Security is an important component of the payment architecture.
The system incorporates mechanisms for:
- Payment packet encryption
- Data integrity verification
- Transaction freshness validation
- Duplicate transaction detection
- Idempotent processing
- Secure transaction settlement
These mechanisms help protect transactions while they are propagated through the offline mesh.
🔄 Idempotency
Offline transactions may reach the backend through multiple bridge devices.
For example:
                 ┌── Bridge A ──┐
                 │              │
Same Payment ────┼── Bridge B ──┼──► Backend
                 │              │
                 └── Bridge C ──┘

The backend must ensure that the same payment is settled only once.
The system therefore uses idempotent transaction processing to prevent duplicate debits.
⚡ Concurrent Processing
Multiple bridge devices may attempt to submit the same transaction at approximately the same time.
The transaction processing layer handles concurrent submissions so that:
Submission 1 ──┐
Submission 2 ──┼──► Transaction Engine ──► Single Settlement
Submission 3 ──┘

Only one valid settlement should affect the account balances.
🛠️ Technology Stack
Technology	Usage
Java 17+	Application development
Spring Boot	Backend framework
Spring Data JPA	Data persistence
H2 Database	In-memory database
Maven	Build and dependency management
REST API	Backend communication
JUnit	Automated testing
Cryptography	Payment security


📂 Project Structure
UPI_Without_Internet/
│
├── .mvn/
│   └── wrapper/
│
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── ...
│   │   │
│   │   └── resources/
│   │       └── ...
│   │
│   └── test/
│       └── java/
│
├── .gitignore
├── mvnw
├── mvnw.cmd
├── pom.xml
└── README.md

💻 Requirements
Before running the application, make sure you have:
- Java JDK 17 or higher
- IntelliJ IDEA
- Git
Verify Java:
java -version

🚀 Installation
Clone the repository:
git clone https://github.com/varad1969/UPI_Without_Internet.git

Move into the project directory:
cd UPI_Without_Internet

▶️ Running the Application
IntelliJ IDEA
1. Open the project in IntelliJ IDEA.
2. Allow Maven to import the dependencies.
3. Configure JDK 17 or higher.
4. Locate the Spring Boot application class.
5. Click the green Run button.
6. Wait for the application to start.
Then open:
http://localhost:8080

Windows PowerShell
.\mvnw.cmd spring-boot:run

Linux / macOS
./mvnw spring-boot:run

🧪 Running Tests
Run all tests using:
Windows
.\mvnw.cmd test

Linux / macOS
./mvnw test

🧪 Concurrency Test
The system includes a concurrency scenario where multiple bridge devices attempt to submit the same payment.
Example:
./mvnw test -Dtest=IdempotencyConcurrencyTest#singlePacketDeliveredByThreeBridgesSettlesExactlyOnce

This verifies that a payment submitted through multiple bridges does not result in multiple settlements.
📊 Example Transaction
Sender:
Alice

Receiver:
Bob

Amount:
₹500

Network:
Offline

        ↓

Payment Packet Created

        ↓

Mesh Propagation

        ↓

Bridge Device

        ↓

Backend Synchronization

        ↓

Transaction Verification

        ↓

₹500 Debited from Alice

        ↓

₹500 Credited to Bob

🌐 API
The application exposes REST endpoints for payment processing, mesh operations, bridge synchronization, and transaction management.
The backend architecture separates:
Controller
    ↓
Service
    ↓
Transaction Processing
    ↓
Repository
    ↓
Database

