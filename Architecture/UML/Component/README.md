## Components Diagram
Use this diagram to view how the components of redbridge logically connect with each other.

### Components
Let's take a look at what each component of redbridge does.

**[redbridge UI](https://redbridge-demo.red.dev)**

The UI is the user interface for redbridge. It is written in HTML and vanilla Javascript to minimize software dependency attack vectors. Anyone can run this software. However, for security, we expect most bridge users will use a version hosted by a trusted entity such as the Zcash Foundation and accessed using HTTPS. The code will be signed.

It is up to each note operator to secure its redbridge UI web server from DoS attacks, etc. We will provide a basic strategy for this that we implemented with open source tools.

**redbridge Agent**

The redbridge Agent is software run by a redbridge guardian that is responsible for performing tasks that facilitate the bridging process. Though software, the redbridge Agent is sometimes shown as an actor (in the shape of a stick figure person) because its job is to initiate actions much like a human would. Any redbridge Agent can perform these task, and the token owner can choose which redbridge Agent they would like to use in the Bridge UI. These tasks are:

- Deliver warp messages
- Get ZEC balances
- Submit signed ZEC transactions
- Initiate guardian maintenance
- Request consensus validation

The redbridge Agent need not be trusted; all operations requiring trust are performed by the RCE. 

It is up to each note operator to secure its redbridge Agent from DoS attacks, etc. We will provide a basic strategy for this that we implemented with the ZAOR.

**redbridge Consensus Engine (RCE)**

The redbridge Consensus Engine performs all bridge operations that require trust, for instance signing transactions, composing and verifying warp messages, and recording bridging operations on the redbridge blockchain. The redbridge blockchain serves by in large as an immutable record of RCE activity.

**redbridge Contract**

The redbridge Contract resides on the Avalanche primary network's C-Chain and is responsible for minting *wrapped* ZEC.rbr for use on this chain and beyond. It is also responsible for preparing warp messages for bridging operations and for burning ZEC.rbr during bridging from Avalanche to Zcash.

### Other Software

redbridge interacts directly or indirectly with the following other software packages that are needed for redbridge to function but are maintained seperately from redbridge components.

**Zebra**

Zebra is Zcash node software maintained by the Zcash Foundation.


**AvalancheGo**

AvalancheGo is Avalanche node software maintained by Ava Labs.

**C-Chain**

The Avalanche primary network's C-Chain which is based on the Ethereum Virtual Machine (EVM).

**P-Chain**

The Avalanche primary network's P-Chain is responsible for adding and removing validators from the primary network and subnets and for storing BLS keys that redbridge uses for warp messaging.

