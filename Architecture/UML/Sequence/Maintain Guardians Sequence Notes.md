## Maintain ZavaX guardians and Wallet Control Sequence Diagram Notes
### Maintain ZavaX guardians - Wallet States
As guardian maintenance progresses, the bridge wallet passes through three distinct *wallet states* (0-2). Which state a transaction is in is important to know during failure recovery.

<b>State 0</b>
- No changes
  
<b>State 1</b>
- Signatures for transfer are ready

<b>State 2</b>
- Wallet transfer is complete


### Failure Recovery
<table>
    <tr>
        <td>Action</td>
        <td>Description</td>
        <td>Wallet State</td>
        <td>Resolution</td>
    </tr>
    <tr>
        <td>1-5</td>
        <td>Comm failure with Avalanche primary network.</td>
        <td>State 0<br>No changes</td>
        <td>Unable to continue, so ZavaX Agent will miss maintenance opportunity.</td>
    </tr>
    <tr>
        <td>6</td>
        <td>Comm failure with ZavaX subnet.</td>
        <td>State 0<br>No changes</td>
        <td>Unable to continue, so ZavaX Agent will miss maintenance opportunity.</td>
    </tr>
    <tr>
        <td>7-11</td>
        <td>ZavaX subnet is down.</td>
        <td>State 0<br>No changes</td>
        <td>ZavaX Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>12</td>
        <td>No quarum of new/continuing guardians.</td>
        <td>State 0<br>No changes</td>
        <td>ZCE will notify ZavaX Agent to try again in 24 hours.</td>
    </tr>
    <tr>
        <td>13</td>
        <td>ZavaX subnet is down.</td>
        <td>State 0<br>No changes</td>
        <td>ZavaX Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>14</td>
        <td>ZavaX Agent comm failure with ZavaX subnet.</td>
        <td>State 0<br>No changes</td>
        <td>ZavaX Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>15-16</td>
        <td>ZavaX Agent comm failure with Zebra.</td>
        <td>State 0<br>No changes</td>
        <td>ZavaX Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>17-22</td>
        <td>ZavaX Agent comm failure with ZavaX subnet.</td>
        <td>State 0 or State 1<br>Signed transfer may be ready</td>
        <td>ZavaX Agent will timeout after one minute, then will check ZavaX chain to see if signatures have been written. If so, it can continue. If not, it logs a failure. (note 3)</td>
    </tr>
    <tr>
        <td>25-27</td>
        <td>ZavaX Agent comm failure with Zebra.</td>
        <td>State 1 or State 2<br>Signed transfer may have been submitted and confirmed</td>
        <td>ZavaX Agent will timeout after one minute and then try again. (note 3).</td>
    </tr>
    <tr>
        <td>28</td>
        <td>ZavaX Agent comm failure with ZavaX subnet.</td>
        <td>State 2<br>Wallet transfer is complete</td>
        <td>Retry once ZavaX subnet is up again.</td>
    </tr>
    <tr>
        <td>29-34</td>
        <td>ZavaX subnet failure.</td>
        <td>State 2<br>Wallet transfer is complete</td>
        <td>Retry once ZavaX subnet is up again.</td>
    </tr>
    <tr>
        <td>35</td>
        <td>ZavaX Agent comm failure with ZavaX subnet.</td>
        <td>State 2<br>Wallet transfer is complete</td>
        <td>Log failure (but no consequences).</td>
    </tr>
<table>


### Notes:

1. With some failures above, as you can see in the Wallet State column, the exact state will not be known at the time of failure or will not have been reported. If wallet maintenance is not yet complete, a new ZavaX Agent will have an opportunity to pick up task as it becomes *certified*, and when it does, it must determine the correct state and proceed accordingly. 

2. As it starts, the UI checks with the ZavaX Agent for previous failures and prompt Owner if there has been one for that Owner's address about how to proceed.

3. Only *certified* ZavaX Agents run by guardians can perform connections 6, 17 and 28 of bridge maintenance, and the ZCE will only accept connections from one certified ZavaX Agent at a time. 
   
4. For the transaction built for connection 17, if someone has previously sent money directly to the bridge wallet address (which is difficult to do but possible), these funds will not be moved to the new wallet. 
5. ZCE = ZavaX Consensus Engine.

###


### Certified ZavaX Agent Rotation Rules

The Maintain guardians task runs daily at midnight UTC. The ZavaX Agent that initiates the Maintain guardians process is paid a fee. For fairness, all current guardians are given an equal chance as follows. 

The ZCE makes a list of guardians. The guardian at the top of the list has the exclusive *certified* opportunity to do it starting at 0:00 UTC. Then after five minutes, two more are certified, then at ten minutes, four more are certified, and so on, until the use-case has been completed for that day or all ZavaX Agents have been certified, whichever comes first.

The next day, the guardian with the ZavaX Agent that completed the bridge maintenance is placed at the bottom of the list, and the rest are moved up one spot.

During the Maintain guardians process, there are three times when a ZavaX Agent must ask the ZCE to perform work:

1. Bringing the bridge down for maintenance and asking the new and continuing guardians to create a new wallet.
   
2. Transferring funds from the old wallet to the new wallet.

3. Cleaning up and bringing the bridge back up. 

Only certified ZavaX Agents may complete these tasks. 