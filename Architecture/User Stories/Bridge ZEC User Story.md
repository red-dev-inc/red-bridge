## **User Story: Bridge ZEC to ZEC.z**

### **As** a ZEC Owner,

**I want to** bridge my ZEC to ZEC.z,

**So that** I can use my assets on the Avalanche C-Chain and beyond.

---

### **Flow**:

1. **Initiating Bridge Process**:
    - **Given** I am a ZEC Owner,
    - **When** I access the ZavaX Bridge UI and connect my AVAX wallet,
    - **Then** the UI requests and display my ZEC.z balance from the Core Wallet and my transparent ZEC balance from the ZavaX Agent.

2. **Preparing for Bridging**:
    - **Given** the UI displays my transparent ZEC balance,
    - **When** I send ZEC to the transparent address associated with my AVAX wallet using Zcash Wallet,
    - **Then** I see a confirmation of the transfer completion and an updated transparent ZEC balance in the UI.

3. **Initiating Bridge Request**:
    - **Given** the transparent ZEC is available for bridging,
    - **When** I confirm the bridging action on the UI,
    - **Then** the UI requests a signature from my Core Wallet and compose a signed ZEC transaction.

4. **Submitting Bridging Transaction**:
    - **Given** a signed ZEC transaction is composed,
    - **When** I verify the transaction (ZIP-317) fee, authorize it and the transaction is submitted to the ZavaX Agent,
    - **Then** the ZavaX Agent forwards this transaction to Zebra for inclusion in the Zcash blockchain.

5. **Transaction Confirmation and ZEC.z Minting**:
    - **Given** the transaction is submitted,
    - **When** the ZavaX Agent confirms the transaction depth with Zebra and submits a validation request to the ZavaX Consensus Engine,
    - **Then** the ZavaX Consensus Engine validates the transaction, record fees, and mint ZEC.z.

6. **Completing the Bridging Process**:
    - **Given** the ZEC.z minting is authorized,
    - **When** the ZavaX Consensus Engine sends the minting message to the ZEC.z Contract,
    - **Then** the contract verifies the threshold signature, mint ZEC.z, and send it to my AVAX wallet address.

7. **Finalizing Transaction and Updating Balance**:
    - **Given** the ZEC.z is minted and sent to my AVAX wallet,
    - **When** the UI requests and receives an updated ZEC.z balance from the Core Wallet,
    - **Then** the UI displays the updated ZEC and ZEC.z balances along with the final results of the bridging process.
