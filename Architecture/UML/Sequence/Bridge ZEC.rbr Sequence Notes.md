## Bridge ZEC.rbr to ZEC Sequence Diagram Notes
### ZEC.rbr to ZEC Bridging States
As each bridging transaction progresses, it passes through five distinct *bridging states* (0-4) with (a) variations on success or (b) variations on failure. It is important to consider which state a transaction is in during failure recovery. The states match the non-Snap flow -- no additional state is introduced because the Snap is only involved for (a) providing the public key at the start and (b) returning the ZEC balance at the end. The actual unwrap is performed by the RCE via FROST, and the unwrapped ZEC lands at the Snap's transparent address on Zcash.

<b>State 0</b>
- Nothing bridged

<b>State 1</b>
- ZEC.rbr is in escrow
- ZEC unwrap warp message ready for pickup

<b>State 2</b>
- ZEC.rbr is in escrow
- ZEC tx is ready for signature assembly and submission

<b>State 3a</b>
- ZEC.rbr is in escrow
- ZEC has been unwrapped to Snap's transparent address

<b>State 3b</b>
- ZEC.rbr is in escrow
- ZEC has not been unwrapped

<b>State 4a</b>
- ZEC.rbr has been burnt
- ZEC has been unwrapped to Snap's transparent address
- Bridging is complete

<b>State 4b</b>
- ZEC.rbr has been returned to ZEC.rbr Owner's primary MetaMask wallet
- ZEC has not been unwrapped
- Bridging failed and original balances of ZEC and ZEC.rbr have been restored

### Failure Recovery

For simplicity and clarity, the sequence diagram does not show what happens in case of comm or system failures of components. The following table (presented in lieu of activity diagrams) describes what can fail at each step, and the resolution strategy. For each row, you can also see what is known at the time of failure regarding the *bridging state*. Successful resolution strategy depends on determining the bridging state of the bridge transaction that has failed.

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
        <td>4-8</td>
        <td>MetaMask or Avalanche C-Chain failure during ZEC.rbr balance query.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser with MetaMask.</td>
    </tr>
    <tr>
        <td>9-10</td>
        <td>Snap fails to return ZEC balance. Lightwalletd sync issue or Snap crash.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser. Reconnect Snap and retry.</td>
    </tr>
    <tr>
        <td>11-12</td>
        <td>UI display or user interaction failures before bridge initiation.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser.</td>
    </tr>
    <tr>
        <td>13-15</td>
        <td>MetaMask plugin failure or no connectivity to Avalanche.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart browser and MetaMask.</td>
    </tr>
    <tr>
        <td>16</td>
        <td>redbridge Agent down.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>UI times out. Check connectivity.</td>
    </tr>
    <tr>
        <td>17-20</td>
        <td>Avalanche network connectivity lost or Avalanche primary network down.</td>
        <td>State 0 or 1<br>ZEC.rbr may be in escrow.</td>
        <td>UI times out. Check connectivity.</td>
    </tr>
    <tr>
        <td>21-22</td>
        <td>redbridge Agent failure or comm failure with Avalanche local node.</td>
        <td>State 0 or 1<br>ZEC.rbr may be in escrow.</td>
        <td>If nothing yet bridged, inform ZEC.rbr Owner and start over. If funds are in escrow, UI informs Owner to retry with the same or a different redbridge Agent to continue bridging. The redbridge Agent picks up warp message from contract and continues bridging.</td>
    </tr>
    <tr>
        <td>23</td>
        <td>redbridge Agent failure or comm failure with redbridge L1.</td>
        <td>State 1<br>ZEC.rbr is in escrow</td>
        <td>redbridge Agent informs UI, UI informs Owner, and then Owner can try again to retry with same or new redbridge Agent.</td>
    </tr>
    <tr>
        <td>24-27</td>
        <td>redbridge L1 is down.</td>
        <td>State 1 or 2<br>ZEC.rbr is in escrow, and FROST signatures may be ready</td>
        <td>redbridge Agent times out and then informs UI, and then Owner can try again to retry with same or new redbridge Agent.</td>
    </tr>
    <tr>
        <td>28</td>
        <td>redbridge Agent failure.</td>
        <td>State 2<br>ZEC.rbr is in escrow, and FROST signatures are ready</td>
        <td>UI times out, and then Owner can try again to retry with same or new redbridge Agent.</td>
    </tr>
    <tr>
        <td>29-31</td>
        <td>redbridge Agent comm failure with Zebra or Zcash down.</td>
        <td>State 2 or 3<br>ZEC.rbr is in escrow, and ZEC may have been unwrapped to Snap's transparent address</td>
        <td>redbridge Agent informs UI, and UI checks to see if transaction went through. If not, informs Owner to retry. If so, retries with same redbridge Agent at 32.</td>
    </tr>
    <tr>
        <td>32</td>
        <td>redbridge Agent comm failure with redbridge L1.</td>
        <td>State 2 or 3<br>ZEC.rbr is in escrow, and ZEC may have been unwrapped</td>
        <td>redbridge Agent times out and informs UI for retry later.</td>
    </tr>
    <tr>
        <td>33-34</td>
        <td>redbridge L1 down or Zcash down (unlikely).</td>
        <td>State 2 or 3<br>ZEC.rbr in escrow, and ZEC may have been unwrapped</td>
        <td>redbridge Agent times out and informs UI for retry later.</td>
    </tr>
    <tr>
        <td>35</td>
        <td>redbridge L1 down (unlikely).</td>
        <td>State 2 or 3a or 3b<br>ZEC.rbr in escrow, and ZEC may have been unwrapped to Snap's transparent address</td>
        <td>redbridge Agent times out and informs UI for retry later with the same or a different redbridge Agent.</td>
    </tr>
    <tr>
        <td>36</td>
        <td>redbridge Agent comm failure with redbridge L1.</td>
        <td>State 2 or 3a or 3b<br>ZEC.rbr in escrow, and ZEC may have been unwrapped</td>
        <td>redbridge Agent times out and informs UI and can try to continue with the same or a different redbridge Agent.</td>
    </tr>
    <tr>
        <td>37</td>
        <td>redbridge Agent comm failure with Avalanche.</td>
        <td>State 3a<br>ZEC.rbr in escrow, and ZEC has been unwrapped to Snap's transparent address</td>
        <td>redbridge Agent times out and informs UI and can try to continue with the same or a different redbridge Agent.</td>
    </tr>
    <tr>
        <td>38-40</td>
        <td>Avalanche down (unlikely).</td>
        <td>State 3a<br>ZEC.rbr in escrow or burned, and ZEC has been unwrapped</td>
        <td>redbridge Agent times out and informs UI and can try to continue with the same or a different redbridge Agent.</td>
    </tr>
    <tr>
        <td>41</td>
        <td>redbridge Agent comm failure with Avalanche.</td>
        <td>State 3b<br>ZEC.rbr in escrow, and ZEC has not been unwrapped</td>
        <td>redbridge Agent times out and informs UI and can try to continue with the same or a different redbridge Agent.</td>
    </tr>
    <tr>
        <td>42-45</td>
        <td>Avalanche down (unlikely).</td>
        <td>State 3b or 4b<br>ZEC.rbr in escrow or returned to primary wallet, and ZEC has not been unwrapped</td>
        <td>redbridge Agent times out and informs UI and can try to bridge again with the same or a different redbridge Agent.</td>
    </tr>
    <tr>
        <td>46-48</td>
        <td>Snap fails to return updated ZEC balance after unwrap.</td>
        <td>State 4a or 4b<br>Bridging is complete or has failed</td>
        <td>ZEC is safe at the Snap's transparent address (State 4a) or original balances are restored (State 4b). Restart UI/browser, reconnect Snap, and retry balance refresh.</td>
    </tr>
    <tr>
        <td>49-52</td>
        <td>Comm problems between UI, MetaMask, Snap, redbridge Agent, and/or C-Chain, so Owner is not shown results.</td>
        <td>State 4a or 4b<br>Bridging is complete or has failed with user funds reverted</td>
        <td>Refresh UI later and correct balances will show.</td>
    </tr>
</table>

### Notes:

1. With some failures above, as you can see in the Bridging State column, the exact state will not be known at the time of failure or will not have been reported by the UI to the Owner. When the UI restarts and retries calling the redbridge Agent, the redbridge Agent must determine the state and proceed accordingly.

2. As it starts, the UI checks with the redbridge Agent for previous failures and prompts the Owner if there has been one for that Owner's address about how to proceed.

3. The only redbridge Agent that gets paid fees for its service is the one that completes the bridging transfer. However, any guardian-operated redbridge Agent can push bridging transactions forward.

4. UI = User Interface. RCE = redbridge Consensus Engine. PCZT = Partially Created Zcash Transaction.

5. The Zcash Snap (ChainSafe WebZjs) handles Zcash key derivation and balance queries. The Snap derives keys at `m/44'/133'/0'/0/0`, producing a secp256k1 keypair that is used for the Zcash transparent address (and also matches the Snap-derived Avalanche C-Chain address, though the C-Chain address is not used in the unwrap flow).

6. **Unlike the wrap flow (ZEC -> ZEC.rbr), the Snap does not sign anything in the unwrap flow.** The ZEC-side transaction is constructed and signed by the RCE using FROST across signing guardians. The Snap only provides its transparent address (derived from the public key) as the destination for the unwrapped ZEC.

7. **No auto-forward is needed.** ZEC.rbr is burned directly from the user's primary MetaMask wallet, and the unwrapped ZEC lands directly at the Snap's transparent address. There is no intermediate Snap-derived C-Chain address involved in this flow.

8. **Public key approach (security):** The Snap provides only its public key (step 2-3). The UI derives the transparent address locally (hash160) for display and for inclusion in the bridge request. When the bridge is initiated, the ZEC destination address in the warp message is the Snap's transparent address. The RCE (and guardians) verify that the destination address in the warp message matches the expected format, but they cannot independently verify it belongs to the requesting user -- this is the same trust model as any bridge where the user provides a destination address.

9. **ZEC balance via Snap:** ZEC balance queries (steps 9-10, 47-48) go through the Snap's lightwalletd sync rather than through the redbridge Agent and Zebra. This simplifies the initial flow and delays Agent involvement until the bridge request is initiated. The Snap's balance view will reflect newly unwrapped ZEC once its lightwalletd sync catches up.

### Differences from Non-Snap Flow

The key differences between this Snap-based flow and the original Bridge ZEC.rbr Sequence are:

1. **Zcash Snap for key management and ZEC balance** -- The ChainSafe Zcash Snap handles Zcash key derivation and balance queries within MetaMask. In the original flow, the ZEC balance was queried via the redbridge Agent + Zebra, and the ZEC destination address was derived server-side from the AVAX wallet address.

2. **Public key flow** -- The Snap provides its public key (steps 2-3), not pre-derived addresses. The UI derives the transparent address locally for display and includes it in the bridge request as the ZEC destination (step 11).

3. **ZEC destination is Snap's transparent address** -- Instead of deriving the ZEC destination from the user's AVAX wallet address server-side, the UI explicitly sends the Snap's transparent address to the RCE through the warp message. This makes the key ownership model symmetric with the wrap flow (same Snap seed controls the ZEC destination).

4. **No Snap signing in this flow** -- Unlike the wrap flow where the Snap signs a PCZT, the unwrap flow does not require the Snap to sign anything. The ZEC transaction is signed by the RCE via FROST. The Snap's only roles are: (a) deriving the destination transparent address, and (b) reporting the updated ZEC balance after unwrap.

5. **No auto-forward step** -- The wrap flow needs an auto-forward because ZEC.rbr lands at the Snap-derived C-Chain address and must be forwarded to the primary MetaMask wallet. In the unwrap flow, ZEC.rbr is burned directly from the primary MetaMask wallet, so no auto-forward is needed on the EVM side.

### Fees and Gas

The ZEC.rbr Owner needs enough AVAX in their primary MetaMask wallet to pay the gas in step *14* to call the redbridge Contract on the C-Chain.

The redbridge Agent extracts its bridging fee in step *22* in ZEC:
- Bridging fee goes to redbridge Agent guardian's ZEC address.
- Zcash tx fee goes to Zcash miners.
- Remaining ZEC goes to the Snap's transparent address.

### Design Choices
You will note that this algorithm puts the ZEC.rbr in escrow in the smart contract, then tries to unwrap the ZEC, and finally circles back to the smart contract to either burn the ZEC.rbr in escrow or return it to the ZEC.rbr Owner depending on the success or failure of the ZEC unwrap.

A simpler option would be to burn the ZEC.rbr, then try to unwrap the ZEC, and only if this fails, circle back to the smart contract and re-mint the ZEC.rbr.

The reason for this design choice is to minimize the attack surface area for where ZEC.rbr is minted to one location, as minting is one of the most dangerous activities of the bridge. However, this does make the code more complicated, which increases the overall attack surface. At this point, we are unclear which way is better.

### User Errors
This section deals with possible mistakes a bridge user can make and the consequences.

**Loses access to Snap seed**

Because the ZEC destination is the Snap's transparent address, if the user loses access to the Snap's seed (e.g., MetaMask wallet reset without seed phrase backup), the unwrapped ZEC at that transparent address is unrecoverable. The user should back up their MetaMask seed phrase, which backs up both the primary wallet and the Snap-derived keys.

**Initiates unwrap before Snap is fully connected**

If the UI allows bridge initiation before the Snap has returned its public key (step 3), the ZEC destination address cannot be derived. The UI should gate the bridge button on successful Snap connection.
