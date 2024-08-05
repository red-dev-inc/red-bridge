## **User Story: Bridge ZEC.z to ZEC**

### **As** a ZEC.z Owner,

**I want to** bridge my ZEC.z to ZEC,

**So that** I can use my assets on the Zcash platform.

---

### **Flow**:

1. **Initiating Bridge Process**:
    - **Given** I am a ZEC.z Owner,
    - **When** I access the ZavaX Bridge UI and connect my AVAX wallet,
    - **Then** the UI requests and display my ZEC.z balance from the Core Wallet and my transparent ZEC balance from the ZavaX Agent.

2. **Preparing for Bridging**:
    - **Given** the UI shows my ZEC.z and ZEC balances,
    - **When** I initiate a bridge request to convert ZEC.z to ZEC,
    - **Then** the UI facilitates the signing of this request and send it to the ZEC.z Contract along with notifying the ZavaX Agent.

3. **Transaction Verification and Escrow**:
    - **Given** the bridge request is submitted,
    - **When** the ZEC.z Contract verifies fund availability and puts funds into escrow,
    - **Then** it creates a warp message for the ZavaX bridge.

4. **Zcash Transaction Creation**:
    - **Given** the warp message is created,
    - **When** the ZavaX Agent submits this message along with an unsigned transaction for ZEC unwrap to the ZavaX Consensus Engine,
    - **Then** the Consensus Engine validates the warp message, and guardians approve and sign the transaction.

5. **Finalizing Zcash Transaction**:
    - **Given** the transaction is approved and signed,
    - **When** the ZavaX Agent submits the signed transaction to Zebra and it reaches the confirmed depth,
    - **Then** the ZavaX Consensus Engine confirms if the transaction is committed and successful.

6. **Burning or Refunding ZEC.z**:
    - **Given** the Zcash transaction is successful,
    - **When** the ZavaX Consensus Engine sends a confirmation message to the ZavaX Agent,
    - **Then** the ZavaX Agent instructs the ZEC.z Contract to burn the corresponding ZEC.z or refund from escrow in case of failure.

7. **Updating Wallet Balances**:
    - **Given** the ZEC.z is burnt or refunded,
    - **When** the UI requests and receives updated balances from the Core Wallet and ZavaX Agent,
    - **Then** it displays the final results, including the updated ZEC and ZEC.z balances.
