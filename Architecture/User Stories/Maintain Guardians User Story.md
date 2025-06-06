## **User Story: Maintain redbridge guardians and Wallet Control**

### **As** a redbridge Agent,

**I want to** help in the promotion of redbridge validators to being guardians and demotion of redbridge validators from being guardians,

**and I want to** help in the transfer of control of the bridge wallet as validators enter and leave the L1,

**So that** I can help maintain the integrity and functionality of the redbridge.

---

### **Flow**:

1. **Monitoring Validator Changes**:
    - **Given** I am the certified redbridge Agent during a specific time slot in a day,
    - **When** I monitor for new and expiring redbridge validators within specific time slots each day,
    - **Then** I request and receive lists of new and expiring validators from the Avalanche P-Chain.

2. **Initiating Guardian Changes**:
    - **Given** I have the lists of validators changing status,
    - **When** I submit a guardian change request to the redbridge Consensus Engine,
    - **Then** the Consensus Engine verifies my certification and the validity of the change request.

3. **Key Generation for New guardians**:
    - **Given** a guardian change is initiated,
    - **When** new and continuing guardians create their FROST keys for the distributed private key system,
    - **Then** each submits their public key part to the redbridge Consensus Engine.

4. **Assembling New Wallet**:
    - **Given** all public key parts are submitted for thew new wallet,
    - **When** the redbridge Consensus Engine checks for a guardian quorum,
    - **Then** it assembles the new public key and from it create a wallet address, writing the results to a block.

5. **Transferring Wallet Control**:
    - **Given** a new wallet address is created,
    - **When** I, as a redbridge Agent, request the current wallet balance from Zebra and submit an unsigned transfer transaction,
    - **Then** the Consensus Engine gathers signatures from current guardians and write these to a block.

6. **Confirming Transaction and Updating Wallet**:
    - **Given** the transfer transaction signatures are ready,
    - **When** I combine these signatures and submit the signed transfer to Zebra,
    - **Then** I monitor the transaction until the required block depth is reached.

7. **Finalizing Guardian and Wallet Update**:
    - **Given** the wallet migration is successful,
    - **When** the redbridge Consensus Engine verifies this and updates the wallet address,
    - **Then** it queues bridge toll fees for expiring guardians and certified redbridge Agents, clean up, and prepare for the next day.

8. **Resuming Normal Operations**:
    - **Given** the wallet and guardian transition is complete,
    - **When** the redbridge Consensus Engine writes a block with the results,
    - **Then** it updates the status to indicate that the redbridge is functioning normally.
