## **User Story: Bridge ZEC.rbr to ZEC**

### **As** a ZEC.rbr Owner,

**I want to** bridge my ZEC.rbr to ZEC,

**So that** I can use my assets on the Zcash platform.

---

### **Flow**:

1. **Initiating Bridge Process**:
    - **Given** I am a ZEC.rbr Owner,
    - **When** I access the redbridge UI and connect my AVAX wallet,
    - **Then** the UI requests and display my ZEC.rbr balance from the Metamask Wallet and my transparent ZEC balance from the redbridge Agent.

2. **Preparing for Bridging**:
    - **Given** the UI shows my ZEC.rbr and ZEC balances,
    - **When** I initiate a bridge request to convert ZEC.rbr to ZEC,
    - **Then** the UI facilitates the signing of this request and send it to the ZEC.rbr Contract along with notifying the redbridge Agent.

3. **Transaction Verification and Escrow**:
    - **Given** the bridge request is submitted,
    - **When** the ZEC.rbr Contract verifies fund availability and puts funds into escrow,
    - **Then** it creates a warp message for the redbridge.

4. **Zcash Transaction Creation**:
    - **Given** the warp message is created,
    - **When** the redbridge Agent submits this message along with an unsigned transaction for ZEC unwrap to the redbridge Consensus Engine,
    - **Then** the Consensus Engine validates the warp message, and guardians approve and sign the transaction.

5. **Finalizing Zcash Transaction**:
    - **Given** the transaction is approved and signed,
    - **When** the redbridge Agent submits the signed transaction to Zebra and it reaches the confirmed depth,
    - **Then** the redbridge Consensus Engine confirms if the transaction is committed and successful.

6. **Burning or Refunding ZEC.rbr**:
    - **Given** the Zcash transaction is successful,
    - **When** the redbridge Consensus Engine sends a confirmation message to the redbridge Agent,
    - **Then** the redbridge Agent instructs the ZEC.rbr Contract to burn the corresponding ZEC.rbr or refund from escrow in case of failure.

7. **Updating Wallet Balances**:
    - **Given** the ZEC.rbr is burnt or refunded,
    - **When** the UI requests and receives updated balances from the Metamask Wallet and redbridge Agent,
    - **Then** it displays the final results, including the updated ZEC and ZEC.rbr balances.
