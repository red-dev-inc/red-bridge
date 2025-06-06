## **User Story: Bridge ZEC to ZEC.rbr**

### **As** a ZEC Owner,

**I want to** bridge my ZEC to ZEC.rbr,

**So that** I can use my assets on the Avalanche C-Chain and beyond.

---

### **Flow**:

1. **Initiating Bridge Process**:
    - **Given** I am a ZEC Owner,
    - **When** I access the redbridge UI and connect my AVAX wallet,
    - **Then** the UI requests and display my ZEC.rbr balance from the Metamask Wallet and my transparent ZEC balance from the redbridge Agent.

2. **Preparing for Bridging**:
    - **Given** the UI displays my transparent ZEC balance,
    - **When** I send ZEC to the transparent address associated with my AVAX wallet using Zcash Wallet,
    - **Then** I see a confirmation of the transfer completion and an updated transparent ZEC balance in the UI.

3. **Initiating Bridge Request**:
    - **Given** the transparent ZEC is available for bridging,
    - **When** I confirm the bridging action on the UI,
    - **Then** the UI requests a signature from my Metamask Wallet and compose a signed ZEC transaction.

4. **Submitting Bridging Transaction**:
    - **Given** a signed ZEC transaction is composed,
    - **When** I verify the transaction (ZIP-317) fee, authorize it and the transaction is submitted to the redbridge Agent,
    - **Then** the redbridge Agent forwards this transaction to Zebra for inclusion in the Zcash blockchain.

5. **Transaction Confirmation and ZEC.rbr Minting**:
    - **Given** the transaction is submitted,
    - **When** the redbridge Agent confirms the transaction depth with Zebra and submits a validation request to the redbridge Consensus Engine,
    - **Then** the redbridge Consensus Engine validates the transaction, record fees, and mint ZEC.rbr.

6. **Completing the Bridging Process**:
    - **Given** the ZEC.rbr minting is authorized,
    - **When** the redbridge Consensus Engine sends the minting message to the ZEC.rbr Contract,
    - **Then** the contract verifies the threshold signature, mint ZEC.rbr, and send it to my AVAX wallet address.

7. **Finalizing Transaction and Updating Balance**:
    - **Given** the ZEC.rbr is minted and sent to my AVAX wallet,
    - **When** the UI requests and receives an updated ZEC.rbr balance from the Metamask Wallet,
    - **Then** the UI displays the updated ZEC and ZEC.rbr balances along with the final results of the bridging process.
