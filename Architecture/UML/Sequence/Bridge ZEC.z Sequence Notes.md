## Bridge ZEC.z to ZEC Sequence Diagram Notes
### ZEC.z to ZEC Bridging States
As each bridging transaction progresses, it passes through five distinct *bridging states* (0-4) with (a) variations on success or (b) variations on failure. It is important to consider which state a transaction is in during failure recovery.

<b>State 0</b>
- Nothing bridged

<b>State 1</b>
- ZEC.z is in escrow
- ZEC unwrap warp message ready for pickup

<b>State 2</b>
- ZEC.z is in escrow
- ZEC tx is ready for signature assembly and submission

<b>State 3a</b>
- ZEC.z is in escrow
- ZEC has been unwrapped

<b>State 3b</b>
- ZEC.z is in escrow
- ZEC has not been unwrapped

<b>State 4a</b>
- ZEC.z has been burnt
- ZEC has been unwrapped
- Bridging is complete 

<b>State 4b</b>
- ZEC.z has been returned
- ZEC has not been unwrapped
- Bridging failed and original balances of ZEC and ZEC.z have been restored

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
        <td>1-10</td>
        <td>Failures are external, UI, or Core.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser with Core.</td>
    </tr>
    <tr>
        <td>11</td>
        <td>UI failure.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser with Core.</td>
    </tr>
    <tr>
        <td>12-14</td>
        <td>Core plugin failure or no connectivity to Avalanche.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart browser and Core.</td>
    </tr>
    <tr>
        <td>15</td>
        <td>ZavaX Agent down. </td>
        <td>State 0<br>Nothing bridged</td>
        <td>UI times out. Check connectivity.</td>
    </tr>
    <tr>
        <td>16-19</td>
        <td>Avalanche network connectivity lost or Avalanche primary network down. </td>
        <td>State 0 or 1<br>ZEC.z may be in escrow.</td>
        <td>UI times out. Check connectivity.</td>
    </tr>
    <tr>
        <td>20-21</td>
        <td>ZavaX Agent failure or comm failure with Avalanche local node. </td>
        <td>State 0 or 1<br>ZEC.z may be in escrow.</td>
        <td>If nothing yet bridged, inform ZEC.z Owner and start over. If funds are in escrow, UI informs Owner to retry with the same or a different ZavaX Agent to continue bridging. The ZavaX Agent picks up warp message from contract and continues bridging.</td>
    </tr>
    <tr>
        <td>22</td>
        <td>ZavaX Agent failure or comm failure with ZavaX subnet.</td>
        <td>State 1<br>ZEC.z is in escrow</td>
        <td>ZavaX Agent informs UI, UI informs Owner, and then Onwer can try again to retry with same or new ZavaX Agent.</td>
    </tr>
    <tr>
        <td>23-26</td>
        <td>ZavaX subnet is down.</td>
        <td>State 1 or 2<br>ZEC.z is in escrow, and BLS signatures may be ready</td>
        <td>ZavaX Agent times out and then inform UI, and then Owner can try again to retry with same or new ZavaX Agent.</td>
    </tr>
    <tr>
        <td>27</td>
        <td>ZavaX Agent failure.</td>
        <td>State 2<br>ZEC.z is in escrow, and BLS signatures are ready</td>
        <td>UI times out, and then Owner can try again to retry with same or new ZavaX Agent.</td>
    </tr>
    <tr>
        <td>28-30</td>
        <td>ZavaX Agent comm failure with Zebra or Zcash down.</td>
        <td>State 2 or 3<br>ZEC.z is in escrow, and ZEC may have been unwrapped</td>
        <td>ZavaX Agent informs UI, and UI checks to see if transaction went through. If not, informs owner to retry. If so, retries with same ZavaX Agent at 31.</td>
    </tr>
    <tr>
        <td>31</td>
        <td>ZavaX Agent comm failure with ZavaX subnet.</td>
        <td>State 2 or 3<br>ZEC.z is in escrow, and ZEC may have been unwrapped</td>
        <td>ZavaX Agent times out and informs UI for retry later.</td>
    </tr>
    <tr>
        <td>32-33</td>
        <td>Zavax Subnet down or Zcash down (unlikely).</td>
        <td>State 2 or 3<br>ZEC.z in escrow, and ZEC may have been unwrapped</td>
        <td>ZavaX Agent times out and informs UI for retry later.</td>
    </tr>
    <tr>
        <td>34</td>
        <td>Zavax Subnet down (unlikely).</td>
        <td>State 2 or 3a or 3b<br>ZEC.z in escrow, and ZEC may have been unwrapped</td>
        <td>ZavaX Agent times out and informs UI for retry later with the same or a different ZavaX Agent.</td>
    </tr>
    <tr>
        <td>35</td>
        <td>ZavaX Agent comm failure with ZavaX subnet.</td>
        <td>State 2 or 3a or 3b<br>ZEC.z in escrow, and ZEC may have been unwrapped</td>
        <td>ZavaX Agent times out and informs UI and can try to continue with the same or a different ZavaX Agent.</td>
    </tr>
    <tr>
        <td>36</td>
        <td>ZavaX Agent comm failure with Avalanche.</td>
        <td>State 3a<br>ZEC.z in escrow, and ZEC has been unwrapped</td>
        <td>ZavaX Agent times out and inform UI and can try to continue with the same or a different ZavaX Agent.</td>
    </tr>
    <tr>
        <td>37-39</td>
        <td>Avalanche down (unlikely).</td>
        <td>State 3a<br>ZEC.z in escrow or burned, and ZEC has been unwrapped</td>
        <td>ZavaX Agent times out and inform UI and can try to continue with the same or a different ZavaX Agent.</td>
    </tr>
    <tr>
        <td>40</td>
        <td>ZavaX Agent comm failure with Avalanche.</td>
        <td>State 3b<br>ZEC.z in escrow, and ZEC has not been unwrapped</td>
        <td>ZavaX Agent times out and inform UI and can try to continue with the same or a different ZavaX Agent.</td>
    </tr>
    <tr>
        <td>41-44</td>
        <td>Avalanche down (unlikely).</td>
        <td>State 3b or 4b<br>ZEC.z in escrow or returned, and ZEC has not been unwrapped</td>
        <td>ZavaX Agent times out and inform UI and can try to bridge again with the same or a different ZavaX Agent.</td>
    </tr>
    <tr>
        <td>45-51</td>
        <td>Comm problems between UI, Core, ZavaX Agent, Zebra, and/or C-Chain, so Owner is not showed results.</td>
        <td>State 4a or 4b<br>Bridging is complete or has failed with user funds reverted</td>
        <td>Refresh UI later and correct balances will show.</td>
    </tr>
</table>

### Notes:

1. With some failures above, as you can see in the Bridging State column, the exact state will not be known at the time of failure or will not have been reported by the UI to the Owner. When the UI restarts and retries calling the ZavaX Agent, the ZavaX Agent must determine the state and proceed accordingly. 

2. As it starts, the UI checks with the ZavaX Agent for previous failures and prompt Owner if there has been one for that Owner's address about how to proceed.

3. The only ZavaX Agent that gets paid fees for its service is the one that completes the bridging transfer. However, any guardian-operated ZavaX Agent can push bridging transactions forward.

4. UI = User Interface. ZCE = ZavaX Consensus Engine.

### Fees and Gas

The ZEC.z owner needs enough AVAX in their wallet to pay the gas in connection *14* to call the ZavaX Contract on the C-chain. 

The ZavaX Agent extracts its bridging fee in connection *22* in ZEC:
- Bridging fee goes to ZavaX Agent guardian's ZEC address.
- Zcash tx fee goes to Zcash miners.
- Remaining ZEC goes to ZEC.z Owner's t-address.

### Design Choices
You will note that this algorithm puts the ZEC.z in escrow in the smart contract, then tries to unwrap the ZEC, and finally circles back to the smart contract to either burn the ZEC.z in escrow or return it to the ZEC.z Owner depending the success or failure of the ZEC unwrap.

A simpler option would be to burn the ZEC.z, then try to unwrap the ZEC, and only if this fails, circle back to the smart contract and re-mint the ZEC.z.

The reason for this design choice is to minimize the attack surface area for where ZEC.z is minted to one location, as minting is one of the most dangerous activities of the bridge. However, this does make the code more complicated, which increases the overall attack surface. At this point, we are unclear which way is better.

