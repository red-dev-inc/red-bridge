## Components Diagram
Use this diagram to view how the components of ZavaX logically connect with each other.

### Components
Let's take a look at what each component of ZavaX does.

**ZavaX Bridge UI**

The UI is the user interface for ZavaX. It is written in HTML and vanilla Javascript to minimize software dependency attack vectors. Anyone can run this software. However, for security, we expect most bridge users will use a version hosted by a trusted entity such as the Zcash Foundation and accessed using HTTPS. The code will be signed.

It is up to each note operator to secure its ZavaX UI web server from DoS attacks, etc. We will provide a basic strategy for this that we implemented with the ZavaX Oracle.

**ZavaX Agent**

The ZavaX Agent is software run by a ZavaX guardian that is responsible for performing tasks that facilitate the bridging process. Any ZavaX Agent can perform these task, and the token owner can choose which ZavaX Agent they would like to use in the Bridge UI. These tasks are:

- Deliver warp messages
- Get ZEC balances
- Submit signed ZEC transactions
- Initiate guardian maintenance
- Request consensus validation

The ZavaX Agent need not be trusted; all operations requiring trust are performed by the ZCE. 

It is up to each note operator to secure its ZavaX Agent from DoS attacks, etc. We will provide a basic strategy for this that we implemented with the ZavaX Oracle.

**ZavaX Consensus Engine (ZCE)**

The ZavaX Consensus Engine performs all bridge operations that require trust, for instance signing transactions, composing and verifying warp messages, and recording bridging operations on the ZavaX blockchain. The ZavaX blockchain serves by in large as an immutable record of ZCE activity.

**ZavaX Contract**

The ZavaX Contract resides on the Avalanche primary network's C-Chain and is responsible for minting *wrapped* ZEC.z for use on this chain and beyond. It is also responsible for preparing warp messages for bridging operations and for burning ZEC.z during bridging from Avalanche to Zcash.

### Other Software

ZavaX interacts directly or indirectly with the following other software packages that are needed for ZavaX bridge to function but are maintained seperately from ZavaX components.

**Zebra**

Zebra is Zcash node software maintained by the Zcash Foundation.


**AvalancheGo**

AvalancheGo is Avalanche node software maintained by Ava Labs.

**C-Chain**

The Avalanche primary network's C-Chain which is based on the Ethereum Virtual Machine (EVM).

**P-Chain**

The Avalanche primary network's P-Chain is responsible for adding and removing validators from the primary network and subnets and for storing BLS keys that ZavaX uses for warp messaging.

