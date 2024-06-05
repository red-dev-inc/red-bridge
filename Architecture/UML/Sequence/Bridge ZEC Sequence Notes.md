## Bridge ZEC to ZEC.z Sequence Diagram Notes
### ZEC to ZEC.z Bridging States
As each bridging transaction progresses, it passes through four distinct *bridging states* (0-3). It is important to consider which state a transaction is in during failure recovery.

<b>State 0</b>
- Nothing bridged

<b>State 1</b>
- ZEC sent to bridge's Zcash wallet

<b>State 2</b>
- ZEC sent to bridge's Zcash wallet 
- ZCE has written block and warp message

<b>State 3</b>
- ZEC sent to bridge's Zcash wallet
- ZCE has written block and warp message
- ZEC.z minted and set to Owner's C-Chain wallet
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
        <td>1-18</td>
        <td>Failures are external, or UI and Core need to be restarted.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or browser with Core.</td>
    </tr>
    <tr>
        <td>19-20</td>
        <td>Core plugin failure.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or ZEC Owner restarts browser with Core.</td>
    </tr>
    <tr>
        <td>21</td>
        <td>UI failure.</td>
        <td>State 0<br>Nothing bridged</td>
        <td>Restart UI and/or ZEC Owner restarts browser with Core.</td>
    </tr>
    <tr>
        <td>22</td>
        <td>Chosen ZavaX Agent is not responding.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>UI times out. User optionally selects a different ZavaX Agent and retries. </td>
    </tr>
    <tr>
        <td>23</td>
        <td>ZavaX Agent's Zebra is not responding.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>ZavaX Agent informs UI of failure which notifies ZEC Owner. User optionally selects a different ZavaX Agent and retries.</td>
    </tr>
    <tr>
        <td>24-25</td>
        <td>Zebra is not responding. Zcash transaction may have gone through, but ZavaX Agent can't tell.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>UI is informed by ZavaX Agent which notifies ZEC Owner. User optionally selects a different ZavaX Agent and retries.</td>
    </tr>
    <tr>
        <td>26</td>
        <td>Comm failure between ZavaX Agent and ZCE.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>UI is informed by ZavaX Agent which notifies ZEC Owner. User optionally selects a different ZavaX Agent and retries. Resume action varies based on bridging state (see above for states).</td>
    </tr>
    <tr>
        <td>27-30</td>
        <td>Entire ZavaX subnet is down.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>ZavaX Agent times out and informs UI. UI informs Owner and manually retries later.</td>
    </tr>
    <tr>
        <td>31-32</td>
        <td>Times out. ZavaX Agent can't communicate with ZCE.</td>
        <td>State 0 or 1<br>ZEC may have been sent</td>
        <td>ZavaX Agent informs UI. UI informs ZEC Owner who manually retries with another ZavaX Agent.</td>
    </tr>
    <tr>
        <td>33</td>
        <td>ZavaX Agent can't communicate with C-Chain.</td>
        <td>State 2<br>ZEC has been sent, and ZCE has created minting warp message</td>
        <td>Inform UI, and UI informs ZEC Owner for manual retry. New ZavaX Agent resumes at step 31.</td>
    </tr>
    <tr>
        <td>34-36</td>
        <td>Avalanche primary network failure.</td>
        <td>State 2 or 3<br>ZEC has been sent, and ZCE has created minting warp message, and ZEC.z may have been sent</td>
        <td>ZavaX Agent times out and informs UI.</td>
    </tr>
    <tr>
        <td>37-44</td>
        <td>UI failure to communicate or plugin failure. Bridging is complete, but ZEC Owner is not informed.</td>
        <td>State 3<br>Bridging is complete</td>
        <td>UI indicates a timeout. User can then refresh UI later.</td>
    </tr>
</table>

### Notes:

1. With some failures above, as you can see in the Bridging State column, the exact state will not be known at the time of failure or will not have been reported by the UI to the Owner. When the UI restarts and retries calling the ZavaX agent, the ZavaX Agent must determine the correct state and proceed accordingly. 

2. As it starts, the UI checks with the ZavaX Agent for previous failures and prompts the Owner if there has been one for that Owner's address for guidance about how to proceed.

3. The only ZavaX Agent that gets paid fees for its service is the one that completes the bridging transfer. However, any guardian-operated ZavaX Agent can push bridging transactions forward.

4. UI = User Interface. ZCE = ZavaX Consensus Engine.
   
5. Actions 19-20 have Core signing a Zcash transaction. Core includes the code needed to sign a Zcash transaction, but not an API for this yet. If Ava Labs does not provide this API, the bridge will have to use an alternate method to sign the Zcash bridging transaction. There are a number of FOSS alternative solutions that we will consider.

### Fees and Gas

The ZEC Owner only needs to own ZEC, not AVAX, to bridge. The ZavaX Agent is required to pay the gas for interacting with the ZavaX Contract, but it in turn receives payment from the ZCE for acting as the Agent for the bridging transaction. 

The ZavaX Agent is paid a fee for acting as the Agent for the bridging transaction. This fee is determined by the ZavaX Agent itself, so different agents may charge different bridging fees. The fee charged can be a percentage of the transaction and/or a flat fee, and it is paid in ZEC.z. 

In connections *29* and *30* the ZA's bridging fees are subtracted from the bridge transaction, and the warp message contains instructions to mint the bridging fee for the ZA guardian and the remaining amount for the ZEC Owner.

ZavaX Agent will also deposit *seed* AVAX in ZEC Owner's ZEC.z wallet. This amount is chosen by the ZA, but we suggest that it should be enough to cover gas for one or two trades on an Avalanche DEX.



### User Errors
This section deals with possible mistakes a bridge user can make and the consequences.

**Sends ZEC straight to bridge address**

The bridge's Zcash wallet addresses and public key are not disclosed in the Bridge UI, and bridge users are warned not to send ZEC directly to the bridge wallet. If a ZEC owner does do this, the ZEC will be considered unrecoverable as ZavaX does not currently have the capability of refunding the ZEC.