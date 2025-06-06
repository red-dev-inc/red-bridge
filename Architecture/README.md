## red·bridge Zcash-Avalanche Bridge Utilizing an Avalanche L1 (v1.1.0)
## Architecture

Here you will find architecture-related documents for the **red·bridge** Zcash-Avalanche bridge (hereafter referred to as "redbridge" in these documents). This bridge uses the technique known as *wrapping* to transport value between the Zcash and Avalanche platforms, wrapping Zcash's native token ZEC into ZEC.rbr which can be used on the Avalanche C-chain and beyond. Likewise, ZEC.rbr owners can bridge their ZEC.rbr to ZEC on the Zcash blockchain.

### Key Features
1. This bridge is unique in that it can be operated permissionlessly using an Avalanche L1 on Avalanche. The L1's validators serve as bridge guardians, deciding collectively, in a decentralized manner, about what transactions can pass over the bridge and what transactions cannot.

2. A key feature of this bridge is that, as with Ava Lab's Bitcoin-Avalanche bridge, the same private key is used for both the bridge user's Zcash t-address and for the Avalanche C-Chain address. Only the bridge user knows this key; redbridge components do not need to know it. Because the bridge user knows the private key, they control both wallets exclusively; no redbridge node or component ever takes possession of their ZEC.rbr tokens.

3. Another key feature of this bridge is that ZEC locked in the bridge can only be moved by the guardians agreeing to do so *together*. The technology that makes this possible is called FROST threshold signature protocol.

The bridge is being built now. These architectural design documents are a step along the way of bringing it to life.

### Audience
In order to understand these architecture at all, you must at least have knowledge of blockchain technology and know the basics about sending and receiving transactions on both Zcash and Avalanche.

Past this, the more you understand about the following topics, the more you'll be able to understand this bridge's architecture:
- Zcash shielded and transparent transactions and addresses
- Zcash wallets such as Ywallet and Zashi
- Zcash proof-of-work
- Zcash transactions and fees
- Zebra
- Avalanche C-chain
- Avalanche C-chain transactions and addresses
- Avalanche transactions, fees, and gas
- Avalanche Metamask Wallet
- Avalanche P-chain
- Avalanche Avalanche L1s
- AvalancheGo
- Avalanche Snowman++ consensus
- Avalanche warp messaging / Interchain messaging (ICM)
- FROST signatures
- UML 2.0
  
These are each briefly described in the **Terms to Know** section, below.
  
### UML

UML is a specification for creating diagrams that convey how software works. We have chosen PlantUML to create these diagrams because it allows us to write instructions about how the elements on each diagram connect with each other, and then the PlantUML rendering engine takes care of the layout. This allows us to focus on content, making revisions easy. Images of all UML diagrams (.png) are provided alongside the PlantUML files (.wsd). You can also learn to view PlantUML diagrams in VSCode by watching [this tutorial](https://youtu.be/xkwJ9GwgZJU?si=OXHAbQdNk-ca63vr) or by visiting [the PlantUML website](https://plantuml.org).

### How to Learn from this Repo

Here is a brief guide about how we recommend you become familiar with redbridge architecture using this repo. (Most folders also contain a README.md file like this one that describe its contents in more detail.)

1. Begin by watching the short screencast videos in the [BTC.b Bridge Example Screencasts folder](BTC.b%20Bridge%20Example%20Screencasts/) showing how the Bitcoin to Avalanche bridge operated by Ava Labs works from a bridge user's point-of-view. From a user's point-of-view, the redbridge will work similarly, but instead of transporting BTC to BTC.b and back, it sends ZEC to ZEC.rbr and back.

2. There are four use cases illustrated in the **redbridge Use Cases** UML diagram ([PlantUML](UML/Usecase/redbridge%20Usecases.wsd)/[image](UML/Usecase/redbridge%20Usecases.png)). For now, focus on the first two. You can then read the [User Stories](User%20Stories/) for these two use cases.

3. Take a look at the two [Deployment UML diagrams](UML/Deployment/) to see how the decentralized bridge is physically configured and connected. Note that the *components* are shown as *rectangles*. These are the actual parts of the bridge that we are building. They work together with the pre-built packages which are shown as *circles*.

4. Look at the **Component UML diagram** ([PlantUML](UML/Component/redbridge%20Components.wsd)/[image](UML/Component/redbridge%20Components.png)) to see how the various components of the system interact logically. 

5. Dive deeper! Look at the [User Stories](User%20Stories/) and [UML Sequence diagrams](UML/Sequence/) for the two primary use cases, **Bridge ZEC Sequence** ([PlantUML](UML/Sequence/Bridge%20ZEC%20Sequence.wsd)/[image](UML/Sequence/Bridge%20ZEC%20Sequence.png)) and **Bridge ZEC.rbr Sequence** ([PlantUML](UML/Sequence/Bridge%20ZEC.rbr%20Sequence.wsd)/[image](UML/Sequence/Bridge%20ZEC.rbr%20Sequence.png)). In the Sequence diagrams, you will see the components and other entities that you already know from the previous diagrams across the top. If you read sequence diagrams from top to bottom, you can follow the software flow between components as the use case is executed. 

* Note that for clarity and simplicity, these diagrams mostly assume success and that all components are functional and communicating well with each other. However, be aware that for each step, we have considered what would happen when, inevitably, there are problems. You can find these documented in table form in an accompanying *notes* documents in the [same folder](UML/Sequence/). (We decided to use this table format instead of creating UML Activity diagrams for each for brevity and clarity.)

1. Look at the **UML Class diagram** ([PlantUML](UML/Class/redbridge%20Classes.wsd)/[image](UML/Class/redbridge%20Classes.png)) to see how each of the major components of the system are structured. The class diagram shows a static view of the system architecture that is needed to support the use cases.

2. Now it's time to learn about how to become a bridge guardian by staking the redbridge's Avalanche L1 staking token, RBR. (All Avalanche L1s require a staking token; this is what allows them to operate permissionlessly.)

* Begin by looking at the **UML Use Case diagram** ([PlantUML](UML/Usecase/redbridge%20Usecases.wsd)/[image](UML/Usecase/redbridge%20Usecases.png)) again, but this time focusing on the third and fourth use cases (which are connected). Then look at the UML Sequence diagrams that you haven't yet examined, **Create redbridge Validator Sequence** ([PlantUML](UML/Sequence/Create%20redbridge%20Validator%20Sequence.wsd)/[image](UML/Sequence/Create%20redbridge%20Validator%20Sequence.png)), and **Maintain guardians Sequence** ([PlantUML](UML/Sequence/Maintain%20guardians%20Sequence.wsd)/[image](UML/Sequence/Maintain%20guardians%20Sequence.png)). The first and third sequence diagrams should be very familiar to you if you are familiar with how Avalanche validators work. The second, *Maintain guardians Sequence*, is unique to redbridge and shows how validators are promoted to guardians and then demoted near the end of their validation period. 
  
* As with the previous use cases, you can look at the associated *notes* documents [in the same folder](UML/Sequence/) for notes, particularly around failure recovery, and you can zoom in on parts of the UML Sequence diagram by viewing the associated [Communication diagrams](UML/Communication/). Finally, you can look again at the **class diagram** ([PlantUML](UML/Class/redbridge%20Classes.wsd)/[image](UML/Class/redbridge%20Classes.png)) to see the structure of the application itself that is needed to support these use cases. 
### Terms to Know

Here are some of the terms that you need to be familiar with to understand the bridge architecture.

**ZEC.rbr** - *Wrapped Zcash*. ZEC.rbr is an ERC-20 token on the Avalanche C-Chain that represents ZEC that has passed over the redbridge from Zcash to Avalanche. It is also compatible with LayerZero bridging protocols.

**ZEC Owner** - The owner of Zcash's currency, ZEC.

**ZEC.rbr Owner** - The owner of ZEC.rbr.

**redbridge Validator** - An Avalanche validator that validates the redbridge L1.

**redbridge Guardian** - A redbridge Validator that helps to control the redbridge wallet and bridging transactions.

**[FROST](https://frost.zfnd.org/index.html)**: Flexible-Round Optimized Schnorr Threshold signature scheme.

**[BLS](https://en.wikipedia.org/wiki/BLS_digital_signature)** - A digital signature scheme used by Avalanche (and also [Ethereum](https://eth2book.info/capella/part2/building_blocks/signatures/)). 

**redbridge UI** - The [frontend UI](https://redbridge-demo.red.dev) (a live working demonstration of how redbridge will work!) for the redbridge, written in HTML and vanilla javascript.

**[Metamask Wallet](https://Metamask.app/)** - A crypto wallet browser extension that can be used with both Avalanche and Zcash.

**redbridge Agent** - A stand-alone golang app that runs on each redbridge Validator and helps to facilitate bridge operations. Primary duties:
- Deliver warp messages
- Get ZEC balances
- Submit signed ZEC transactions
- Initiate guardian maintenance
- Request consensus validation

**Certified redbridge Agent** - A redbridge Agent operated by the redbridge Validator that is  *certified* by the RCE during a limited time window to perform the redbridge Agent job for the *Maintain redbridge guardians* use case. This use case runs once every 24 hour epoch. As it begins, one redbridge Agent is certified. Then after one minute, two more are certified, then at three minutes, four more are certified, and so on, until the use case has been completed for that day or all redbridge Agents have been certified, whichever comes first.

**ZEC.rbr Contract** - The LayerZero compatible ERC-20 contract on the Avalanche C-Chain that is responsible for redbridge bridging operations including minting, burning, holding in escrow, sending, and receiving ZEC.rbr.

**Avalanche Mainnet** - The production Avalanche platform, also known just as *Avalanche*.

**Avalanche Fuji Testnet** - The test environment for Avalanche that runs the same node software as the Avalanche Mainnet but uses test-AVAX as its primary token which has no value.

**Avalanche Primary Network** - The main Avalanche L1 that contains the P-Chain, the C-Chain, and the X-Chain. (Note: Both the Avalanche Mainnet and the Avalanche Fuji Testnet have their own Avalanche primary network.)

**C-Chain** The Avalanche primary network's C-Chain which is based on the Ethereum Virtual Machine (EVM).

**P-Chain** The Avalanche primary network's P-Chain which is responsible for adding and removing validators from the primary network and Avalanche L1s and for storing BLS keys that redbridge uses for warp messaging.

**Avalanche X-Chain** - A chain used mostly for sending AVAX quickly and cheaply. 

**AvalancheGo** - Avalanche node software maintained by Ava Labs.

**[Zebra](https://zfnd.org/zebra/)** - Zcash node software maintained by the Zcash Foundation.

**[FROST](https://zfnd.org/frost/)** - Threshold signature protocol software maintained by the Zcash Foundation.

**[Ywallet](https://ywallet.app/)** and **[Zashi](https://electriccoin.co)** - Wallet software that can be used with Zcash shielded and transparent pools. (Other wallets exist as well.)

**redbridge Consensus Engine (RCE)** - The heart of the redbridge L1, this code is what allows all the nodes to reach consensus on accepting or rejecting each step required to bridge between Zcash and Avalanche and to perform other bridge-related functionality. All of the decision-making for the bridge happens in this code. No other components have any decision-making power.

**Warp Message** - This is a standard message format that Avalanche supports for communication between Avalanche L1s.

**Gas** - This is the charge, in AVAX, of running code on the C-Chain, and specifically the redbridge contract.

**Tx Fee** - This is the charge, in ZEC, of processing a transaction on the Zcash blockchain. Zcash has variable fees depending on the inputs and output produced by the transaction. If you'd like to know more about the, this fee mechanism is specified in [ZIP-317](https://zips.z.cash/zip-0317).

**Bridging Fee** - This is a charge, in ZEC, ZEC.rbr, or AVAX, that the bridge nodes charge for processing bridging transactions in addition to the Gas and Tx Fees.

**redbridge L1** - This is a group of Avalanche validators that have installed the redbridge components necessary to help run the redbridge L1. The validators are allowed to validate the redbridge L1 just by staking RBR rather than by being on a permissioned list. 

**RBR token** - The redbridge-specific ERC-20 token, created on the C-Chain, that is used for staking the redbridge Avalanche L1.
