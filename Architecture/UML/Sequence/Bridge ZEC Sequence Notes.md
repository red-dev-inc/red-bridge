## Bridge ZEC to ZEC.rbr Sequence Diagram Notes
### ZEC to ZEC.rbr Bridging States
As each bridging transaction progresses, it passes through five distinct *bridging states* (0-4). It is important to consider which state a transaction is in during failure recovery. State 4 is new compared to the non-Snap flow, reflecting the auto-forward step.

<b>State 0</b>
- Nothing bridged

<b>State 1</b>
- ZEC sent from Snap's transparent address to bridge escrow (signed PCZT submitted and confirmed on Zcash)

<b>State 2</b>
- ZEC sent to bridge escrow
- RCE has written block and warp message

<b>State 3</b>
- ZEC sent to bridge escrow
- RCE has written block and warp message
- ZEC.rbr minted to Snap-derived C-Chain address

<b>State 4</b>
- ZEC sent to bridge escrow
- RCE has written block and warp message
- ZEC.rbr minted to Snap-derived C-Chain address
- ZEC.rbr auto-forwarded to Owner's MetaMask primary wallet
- Bridging complete

### Failure Recovery

For simplicity and clarity, the sequence diagram does not show what happens in case of comm or system failures of components. The following table (presented in lieu of activity diagrams) describes what can fail at each step, and the resolution strategy. For each row, you can also see what is known at the time of failure regarding the *bridging state*. Successful resolution depends on determining the bridging state of the bridge transaction that has failed.

<table>
    <tr>
        <td>Action</td>
        <td>Description</td>
        <td>Bridging State</td>
        <td>Resolution</td>
    </tr>
    <tr>
        <td>1-3</td>
        <td>Snap connection or public key retrieval fails.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser. Ensure Zcash Snap is installed in MetaMask.</td>
    </tr>
    <tr>
        <td>4-7</td>
        <td>MetaMask or Avalanche C-Chain failure during ZEC.rbr balance query.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser with MetaMask.</td>
    </tr>
    <tr>
        <td>8-9</td>
        <td>Snap fails to return ZEC balance. Lightwalletd sync issue or Snap crash.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser. Reconnect Snap and retry.</td>
    </tr>
    <tr>
        <td>10-13</td>
        <td>UI derives transparent address and displays it. User sends ZEC from external Zcash wallet to Snap's transparent address.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser. External deposit can be retried independently from the external Zcash wallet.</td>
    </tr>
    <tr>
        <td>14-15</td>
        <td>Snap fails to sync and return updated ZEC balance after deposit.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser. Reconnect Snap and retry sync. ZEC is safe at Snap's transparent address.</td>
    </tr>
    <tr>
        <td>16-17</td>
        <td>UI display or user interaction failures before bridge initiation.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser.</td>
    </tr>
    <tr>
        <td>18-21</td>
        <td>PCZT construction failure. Agent cannot derive addresses from public key or build the unsigned PCZT.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>UI is informed by Agent. User optionally selects a different redbridge Agent and retries.</td>
    </tr>
    <tr>
        <td>22-24</td>
        <td>Snap PCZT signing failure. Snap crashes, user rejects snap_dialog, or communication error.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser. Retry signing with the Snap.</td>
    </tr>
    <tr>
        <td>25</td>
        <td>Chosen redbridge Agent is not responding after signed transaction submitted.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>UI times out. User optionally selects a different redbridge Agent and retries.</td>
    </tr>
    <tr>
        <td>26</td>
        <td>redbridge Agent's Zebra is not responding.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>redbridge Agent informs UI of failure which notifies ZEC Owner. User optionally selects a different redbridge Agent and retries.</td>
    </tr>
    <tr>
        <td>27-28</td>
        <td>Zebra is not responding. Zcash transaction may have gone through, but redbridge Agent can't tell.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>UI is informed by redbridge Agent which notifies ZEC Owner. User optionally selects a different redbridge Agent and retries.</td>
    </tr>
    <tr>
        <td>29</td>
        <td>Comm failure between redbridge Agent and RCE.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>UI is informed by redbridge Agent which notifies ZEC Owner. User optionally selects a different redbridge Agent and retries. Resume action varies based on bridging state.</td>
    </tr>
    <tr>
        <td>30-33</td>
        <td>Entire redbridge L1 is down.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>redbridge Agent times out and informs UI. UI informs Owner and manually retries later.</td>
    </tr>
    <tr>
        <td>34-35</td>
        <td>Times out. redbridge Agent can't communicate with RCE.</td>
        <td>State 1 or 2<br>ZEC has been sent, warp message may exist</td>
        <td>redbridge Agent informs UI. UI informs ZEC Owner who manually retries with another redbridge Agent.</td>
    </tr>
    <tr>
        <td>36</td>
        <td>redbridge Agent can't communicate with C-Chain to relay minting message.</td>
        <td>State 2<br>ZEC has been sent, and RCE has created minting warp message</td>
        <td>Inform UI, and UI informs ZEC Owner for manual retry. New redbridge Agent resumes at step 34.</td>
    </tr>
    <tr>
        <td>37-39</td>
        <td>Avalanche primary network failure during minting.</td>
        <td>State 2 or 3<br>ZEC has been sent, and RCE has created minting warp message, ZEC.rbr may have been minted</td>
        <td>redbridge Agent times out and informs UI.</td>
    </tr>
    <tr>
        <td>40-41</td>
        <td>Communication failure between Agent, UI, and Snap after minting.</td>
        <td>State 3<br>ZEC.rbr minted to Snap-derived address but not yet forwarded</td>
        <td>ZEC.rbr is safe at the Snap-derived address. User restarts UI/browser, reconnects Snap, and retriggers auto-forward.</td>
    </tr>
    <tr>
        <td>42-45</td>
        <td>Snap cannot communicate with MetaMask provider to get primary wallet address or query balance.</td>
        <td>State 3<br>ZEC.rbr minted but not forwarded</td>
        <td>ZEC.rbr is safe at Snap-derived address. Restart browser/MetaMask and retry auto-forward.</td>
    </tr>
    <tr>
        <td>46-47</td>
        <td>Snap fails to construct or sign the auto-forward transaction.</td>
        <td>State 3<br>ZEC.rbr minted but not forwarded</td>
        <td>ZEC.rbr is safe at Snap-derived address. Restart browser and retry auto-forward.</td>
    </tr>
    <tr>
        <td>48-50</td>
        <td>UI fails to broadcast the signed auto-forward transaction, or C-Chain rejects it.</td>
        <td>State 3<br>ZEC.rbr minted but not forwarded</td>
        <td>ZEC.rbr is safe at Snap-derived address. UI can retry broadcast. If gas issue, Agent may need to send additional AVAX to Snap-derived address.</td>
    </tr>
    <tr>
        <td>51-57</td>
        <td>UI failure to communicate or plugin failure. Bridging is complete, but ZEC Owner is not informed.</td>
        <td>State 4<br>Bridging is complete</td>
        <td>UI indicates a timeout. User can then refresh UI later.</td>
    </tr>
</table>

### Notes:

1. With some failures above, as you can see in the Bridging State column, the exact state will not be known at the time of failure or will not have been reported by the UI to the Owner. When the UI restarts and retries calling the redbridge agent, the redbridge Agent must determine the correct state and proceed accordingly.

2. As it starts, the UI checks with the redbridge Agent for previous failures and prompts the Owner if there has been one for that Owner's address for guidance about how to proceed.

3. The only redbridge Agent that gets paid fees for its service is the one that completes the bridging transfer. However, any guardian-operated redbridge Agent can push bridging transactions forward.

4. UI = User Interface. RCE = redbridge Consensus Engine. PCZT = Partially Created Zcash Transaction.

5. The Zcash Snap (ChainSafe WebZjs) handles all Zcash key operations. The Snap derives keys at `m/44'/133'/0'/0/0`, producing a secp256k1 keypair that is used for both the Zcash transparent address and the Snap-derived Avalanche C-Chain address.

6. The Snap's `endowment:ethereum-provider` is **read-only** -- the Snap cannot broadcast transactions via MetaMask's Ethereum provider. Instead, the Snap signs transactions internally and returns raw signed bytes to the bridge dApp, which broadcasts via direct RPC.

7. The Snap queries ZEC balance via its built-in lightwalletd sync (gRPC-web proxy). This is used for display purposes in the UI. The redbridge Agent uses its own Zebra full node for operational needs (PCZT construction, transaction submission, depth polling, validation).

8. **Public key approach (security):** The Snap provides only its public key to the UI (step 2-3). The UI derives the transparent address locally (hash160) for display purposes. When the bridge is initiated, the UI passes the public key to the Agent (step 18). The Agent independently derives both the transparent address and C-Chain address from the public key (step 19). This ensures the "same key" property cannot be broken by a compromised UI -- a malicious dApp cannot substitute a different C-Chain minting target since the Agent derives it directly from the Snap's public key.

9. **Silent auto-forward:** The auto-forward (steps 40-50) does not require user approval via snap_dialog. The Snap signs the forwarding transaction silently. This is acceptable because: (a) the user already consented to the bridge at step 23 (PCZT signing), (b) the auto-forward moves the user's own tokens to the user's own MetaMask wallet, and (c) no funds leave the user's control. This makes the Snap-derived address an invisible implementation detail from the user's perspective.

### Differences from Non-Snap Flow

The key differences between this Snap-based flow and the original Bridge ZEC Sequence are:

1. **Zcash Snap for key management and signing** -- The ChainSafe Zcash Snap handles key derivation, PCZT signing, and ZEC balance queries within MetaMask. The separate Zcash Wallet participant is retained for the external ZEC deposit step.

2. **Public key flow** -- The Snap provides its public key (steps 2-3), not pre-derived addresses. The UI derives the transparent address locally for display (step 10). The Agent independently derives both the transparent address and C-Chain address from the public key (step 19), ensuring the "same key" security property is verified server-side.

3. **ZEC balance via Snap** -- ZEC balance queries (steps 8-9, 14-15, 55-56) go through the Snap's lightwalletd sync rather than through the redbridge Agent and Zebra. This simplifies the initial flow and delays Agent involvement until the bridge request is initiated.

4. **PCZT construction by Agent** -- The redbridge Agent derives addresses from the public key and constructs the unsigned PCZT (steps 18-21), which the UI relays to the Snap for signing. In the original, the UI composed the signed ZEC transaction after getting a MetaMask signature.

5. **Auto-forward group (steps 40-50)** -- A new group that silently transfers ZEC.rbr from the Snap-derived C-Chain address to the user's MetaMask primary wallet. No user approval is needed -- the bridge approval at step 23 implicitly covers it. This is necessary because the Snap-derived address (`m/44'/133'`) differs from the user's MetaMask address (`m/44'/60'`).

6. **New bridging State 4** -- The auto-forward step introduces a new intermediate state. At State 3, ZEC.rbr has been minted but sits at the Snap-derived address. Failures here are safe -- ZEC.rbr is recoverable and the auto-forward can be retried.

### Fees and Gas

The ZEC Owner only needs to own ZEC, not AVAX, to bridge. The redbridge Agent is required to pay the gas for interacting with the redbridge Contract, but it in turn receives payment from the RCE for acting as the Agent for the bridging transaction.

The redbridge Agent is paid a fee for acting as the Agent for the bridging transaction. This fee is determined by the redbridge Agent itself, so different agents may charge different bridging fees. The fee charged can be a percentage of the transaction and/or a flat fee, and it is paid in ZEC.rbr.

In the validation steps, the RA's bridging fees are subtracted from the bridge transaction, and the warp message contains instructions to mint the bridging fee for the RA guardian and the remaining amount for the ZEC Owner.

redbridge Agent will also deposit *seed* AVAX to the Snap-derived C-Chain address (step 36). This seed AVAX covers:
- Gas for the auto-forward ERC-20 transfer (Snap-derived address to MetaMask primary)
- Enough remaining for one or two trades on an Avalanche DEX (sent to MetaMask primary along with ZEC.rbr)

### User Errors
This section deals with possible mistakes a bridge user can make and the consequences.

**Sends ZEC straight to bridge escrow address**

The bridge's Zcash escrow addresses and public key are not disclosed in the Bridge UI, and bridge users are warned not to send ZEC directly to the bridge wallet. If a ZEC owner does do this, the ZEC will be considered unrecoverable as redbridge does not currently have the capability of refunding the ZEC.

**Sends ZEC to wrong address**

The user must copy the Snap's transparent address from the bridge UI when depositing ZEC from their external Zcash wallet. If they send ZEC to a different address, the bridge cannot detect or recover the funds.
