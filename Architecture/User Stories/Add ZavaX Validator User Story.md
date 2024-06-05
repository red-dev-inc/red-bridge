## **User Story: Adding a ZavaX Validator**

### **As** a ZAX Owner,

**I want to** initiate the process of becoming a ZavaX subnet validator through the Avalanche command line interface,

**So that** I can be a part of the ZavaX validator network, in order to later become a ZavaX bridge guardian, contributing to the security and operation of the ZavaX bridge.

---

### **Flow**:

1. **Initialization**:
    - **Given** I am a ZAX Owner,
    - **When** I initiate the validation process through the Avalanche-cli,
    - **Then** the Avalanche-cli requests a transaction signature from my Hardware Wallet device.

2. **Hardware wallet Interaction**:
    - **Given** a signature request is sent to the hardware wallet (HW) device ,
    - **When** my HW device prompts me for approval,
    - **And** I approve the transaction,
    - **Then** my HW device signs the transaction.

3. **Completion of Validation Request**:
    - **Given** the HW wallet has signed the transaction,
    - **When** the signed transaction is returned to the Avalanche-cli,
    - **And** the Avalanche-cli submits the validation request to the Avalanche P-Chain,
    - **Then** I receive a response indicating the status of my validation request.

