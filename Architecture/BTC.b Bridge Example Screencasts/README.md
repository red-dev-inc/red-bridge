## BTC.b Bridge Example Screencasts
These two screencasts show the process of transferring BTC from the bitcoin platform to BTC.b on the Avalanche platform using the **Bitcoin-Avalanche Bridge** built into Core wallet.

The ZavaX bridge works similarly, so seeing how this bridge works will help you understand the ZavaX bridge. 

### Similarities

1. Both bridges use a *wrapping* process to bridge native assets from the *origin* platform (bitcoin or Zcash) to the *destination* platform (Avalanche). Native assets are deposited into a wallet controlled by the bridge guardians on the *origin* platform (BTC or ZEC), and then a *wrapped* or synthetic version of the same asset is minted on the *destination* platform (BTC.b or ZEC.z).
2. Similarly, both bridges use an *unwrapping* process to bridge assets from the *destination* platform (Avalanche) back to the *origin* platform (bitcoin or Zcash). The *wrapped* or synthetic version of the asset is burned on the *destination* platform (BTC.b or ZEC.z), and then native asset (BTC or ZEC) is returned to the user from the bridge wallet on the *origin* platform.
3. The user experience is similar. For instance, in order to use the bridge from *origin* to *destination*, the user must first transfer assets into an address linked to the owner's Avalanche private key. (In these screencasts, the Avalanche C-Chain address ending in *1690* is controlled by the same private key as the Zcash t-address ending in *zsd*, and ZEC must be transfered to the *zsd* address in preparation to bridge it to Avalanche.)

### Differences

The differences are largely behind-the-scenes, in the backend.
1. The ZavaX bridge guardians are validators of the ZavaX elastic subnet on the Avalanche platform. Anyone who holds at least 2000 ZAX can stake this ZAX to run a validator and then become a bridge guardian. This is different from the **Bitcoin-Avalanche Bridge** where eight guardians have been selected by Ava Labs.
2. The Bitcoin-Avalanche bridge uses a single computer's [Intel SGX](https://en.wikipedia.org/wiki/Software_Guard_Extensions) enclave to secure bridge operations. the ZavaX bridge replaces the SGX enclave with the subnet's distributed ZavaX Consensus Engine (ZCE) which serves the same security function and is more robust because it is decentralized.