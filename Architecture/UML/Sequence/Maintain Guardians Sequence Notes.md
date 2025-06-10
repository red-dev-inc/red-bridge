## Maintain redbridge guardians and Wallet Control Sequence Diagram Notes
### Maintain redbridge guardians - Wallet States
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
        <td>Unable to continue, so redbridge Agent will miss maintenance opportunity.</td>
    </tr>
    <tr>
        <td>6</td>
        <td>Comm failure with redbridge L1.</td>
        <td>State 0<br>No changes</td>
        <td>Unable to continue, so redbridge Agent will miss maintenance opportunity.</td>
    </tr>
    <tr>
        <td>7-11</td>
        <td>redbridge L1 is down.</td>
        <td>State 0<br>No changes</td>
        <td>redbridge Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>12</td>
        <td>No quarum of new/continuing guardians.</td>
        <td>State 0<br>No changes</td>
        <td>RCE will notify redbridge Agent to try again in 24 hours.</td>
    </tr>
    <tr>
        <td>13</td>
        <td>redbridge L1 is down.</td>
        <td>State 0<br>No changes</td>
        <td>redbridge Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>14</td>
        <td>redbridge Agent comm failure with redbridge L1.</td>
        <td>State 0<br>No changes</td>
        <td>redbridge Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>15-16</td>
        <td>redbridge Agent comm failure with Zebra.</td>
        <td>State 0<br>No changes</td>
        <td>redbridge Agent will timeout and log failure. (note 3)</td>
    </tr>
    <tr>
        <td>17-22</td>
        <td>redbridge Agent comm failure with redbridge L1.</td>
        <td>State 0 or State 1<br>Signed transfer may be ready</td>
        <td>redbridge Agent will timeout after one minute, then will check redbridge chain to see if signatures have been written. If so, it can continue. If not, it logs a failure. (note 3)</td>
    </tr>
    <tr>
        <td>25-27</td>
        <td>redbridge Agent comm failure with Zebra.</td>
        <td>State 1 or State 2<br>Signed transfer may have been submitted and confirmed</td>
        <td>redbridge Agent will timeout after one minute and then try again. (note 3).</td>
    </tr>
    <tr>
        <td>28</td>
        <td>redbridge Agent comm failure with redbridge L1.</td>
        <td>State 2<br>Wallet transfer is complete</td>
        <td>Retry once redbridge L1 is up again.</td>
    </tr>
    <tr>
        <td>29-34</td>
        <td>redbridge L1 failure.</td>
        <td>State 2<br>Wallet transfer is complete</td>
        <td>Retry once redbridge L1 is up again.</td>
    </tr>
    <tr>
        <td>35</td>
        <td>redbridge Agent comm failure with redbridge L1.</td>
        <td>State 2<br>Wallet transfer is complete</td>
        <td>Log failure (but no consequences).</td>
    </tr>
<table>


### Notes:

1. With some failures above, as you can see in the Wallet State column, the exact state will not be known at the time of failure or will not have been reported. If wallet maintenance is not yet complete, a new redbridge Agent will have an opportunity to pick up task as it becomes *certified*, and when it does, it must determine the correct state and proceed accordingly. 

2. As it starts, the UI checks with the redbridge Agent for previous failures and prompt Owner if there has been one for that Owner's address about how to proceed.

3. Only *certified* redbridge Agents run by guardians can perform connections 6, 17 and 28 of bridge maintenance, and the RCE will only accept connections from one certified redbridge Agent at a time. 
   
4. For the transaction built for connection 17, if someone has previously sent money directly to the bridge wallet address (which is difficult to do but possible), these funds will not be moved to the new wallet. 
5. RCE = redbridge Consensus Engine.

###


### Certified redbridge Agent Rotation Rules

The Maintain guardians task runs daily at midnight UTC. The redbridge Agent that initiates the Maintain guardians process is paid a fee. For fairness, all current guardians are given an equal chance as follows. 

The RCE makes a list of guardians. The guardian at the top of the list has the exclusive *certified* opportunity to do it starting at 0:00 UTC. Then after five minutes, two more are certified, then at ten minutes, four more are certified, and so on, until the use case has been completed for that day or all redbridge Agents have been certified, whichever comes first.

The next day, the guardian with the redbridge Agent that completed the bridge maintenance is placed at the bottom of the list, and the rest are moved up one spot.

During the Maintain guardians process, there are three times when a redbridge Agent must ask the RCE to perform work:

1. Bringing the bridge down for maintenance and asking the new and continuing guardians to create a new wallet.
   
2. Transferring funds from the old wallet to the new wallet.

3. Cleaning up and bringing the bridge back up. 

Only certified redbridge Agents may complete these tasks. 