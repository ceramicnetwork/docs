# 3ID Connect

3ID Connect is the most widely used DID provider for the [3ID DID Method](./method.md). The 3ID Connect authentication SDK allows users to use a 3ID DID with their existing blockchain wallet(s). 3ID Connect was designed for developers building Ceramic-enabled web apps whose users already have a blockchain/crypto wallet. By using 3ID Connect, your users get to rely on the signing and key management capabilities of the crypto wallet(s) they already have instead of needing to install a separate wallet. *3ID Connect is not a substitute for a crypto wallet. It works alongside these wallets.*

[**Installation**](#installation){: .md-button .md-button--primary }   [Demo](https://self.id)

#### Supported blockchains

- Ethereum
- EVM compatible L1s and L2s (i.e. Avalanche, Moonbeam)
- NEAR (coming soon)
- EOS (coming soon)
- Cosmos (coming soon)
- Polkadot (coming soon)

> Want to add a new blockchain? Follow this [**tutorial**](https://blog.ceramic.network/add-authentication-with-new-blockchains-in-3id-connect/) to make a PR.

## **Benefits**

### User experience

- **Nothing to install**: Users don't need to install 3ID Connect. They only need a blockchain wallet.
- **Same identity across accounts**: Users can use the same DID across many different blockchain accounts.
- **Same identity across chains**: Users can use the same DID across all L1 and L2 blockchain platforms.
- **No additional key management**: Users don't need to manage the private key for their DID – just their existing blockchain wallet.
- **Automatic 3Box profile migration**: Users' 3Box profiles will automatically be migrated by 3ID Connect the first time they use 3ID Connect via an application deployed on Ceramic Mainnet. Learn more about the 3Box profiles migration [here](./3box-migration.md).

### Developer experience

- **Easy to install**: See usage section below to add 3ID Connect to your project.
- **3ID provider**: Access all methods in the 3ID DID provider.
- **Better user experience**: User-friendly design makes it seamless for your users to use DIDs.
- **Works with web apps**: Written in JavaScript/TypeScript and hosted in an iFrame.

## **Installation**

Before installing 3ID Connect, you must have [installed a Ceramic client](../../build/javascript/installation.md). By following the steps below, your users will be able to [perform writes](../../build/javascript/writes.md) on Ceramic using a 3ID DID with their blockchain wallet.


### 1. Install from npm

```
npm install @3id/connect
```

### 2. Import the provider

```
import { ThreeIdConnect,  <BlockchainAuthProvider> } from '@3id/connect'
```

??? warning "Understanding `BlockchainAuthProvider`"

    The `BlockchainAuthProvider` parameter is always required but the name shown here is just a placeholder. In your application, you should substitute in the specific BlockchainAuthProvider you are using. A full list of supported BlockchainAuthProviders can be found [here](https://github.com/ceramicnetwork/js-3id-blockchain-utils/tree/master/src/blockchains).

Example using an Ethereum wallet:

```
import { ThreeIdConnect,  EthereumAuthProvider } from '@3id/connect'
```

### 3. Request user's address

```
const addresses = await <blockchainName>.enable()
```

Example using an Ethereum wallet:

```
const addresses = await window.ethereum.enable()
```

### 4. Request authentication

```
const threeIdConnect = new ThreeIdConnect()
const authProvider = new <BlockchainAuthProvider>(<blockchainName>, addresses[0])
await threeIdConnect.connect(authProvider)
```

Example using an Ethereum wallet:

```
const authProvider = new EthereumAuthProvider(window.ethereum, addresses[0])
await threeIdConnect.connect(authProvider)
```
  
  
!!! warning ""

    This will prompt the user with a 3ID Connect permissions window.
  
  
### 5. Create provider instance

```
const provider = await threeIdConnect.getDidProvider()
```

### 6. Set the provider to Ceramic
Set the Provider instance to the DID instance used by your Ceramic client in order to perform writes. You should have configured the DID instance when you [installed your client](../../build/javascript/installation.md).

```
ceramic.did.setProvider(provider)
```

### 7. Authenticate the 3ID

```
await ceramic.did.authenticate()
```

!!! warning ""

    This will prompt the user with a 3ID Connect permissions window.
    
Your users will now be authenticated and can perform [writes](../../build/javascript/writes.md) to streams on Ceramic.

## **Next steps: Writes**

After authenticating with 3ID Connect, users will now be able to perform [writes](../../build/javascript/writes.md).


## **Underlying technologies**

3ID Connect is built on open source technologies and standards. Learn more about the technologies that make 3ID Connect possible.

- [3ID DID Provider](./provider.md): For creating decentralized identifiers and authenticating to Ceramic
- [IDX (CIP-11)](../../tools/identity/idx.md): Protocol and SDK for storing data streams controlled by the user's 3ID
- [3ID Keychain (CIP-20)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-20/CIP-20.md): IDX [definition](../../tools/identity/idx.md#definitions) and schema for using a [TileDocument StreamType](../../streamtypes/tile-document/overview.md) to store a list of encrypted authentication secrets for each wallet account
- [CAIP10Link (CIP-7)](../../streamtypes/caip-10-link/overview.md): StreamType for publicly linking a wallet account to a 3ID DID
- [Crypto accounts (CIP-21)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md): IDX definition and schema for using a TileDocument StreamType to store a list of streamIDs for a DID's CAIP10Links
    
## **Maintainers**
3ID Connect is maintained by [3Box Labs](https://3boxlabs.com).


</br>
</br>
</br>
