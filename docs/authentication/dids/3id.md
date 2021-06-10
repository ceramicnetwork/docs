# 3ID DID

3ID is one of a few DID methods supported by Ceramic that can be used to perform authenticated writes to streams that require DIDs. This includes the two default StreamTypes: Tile Documents and CAIP-10 Links.

When 3IDs are used in conjunction with IDX and the 3ID Keychain (as is implemented in 3ID Connect), a 3ID can easily be controlled with any number of blockchain accounts from any L1 or L2 network. This provides a way to unify a user's identity across all other platforms.

## Usage

See the [Authentication](https://developers.ceramic.network/build/authentication/) page to learn how to use the 3ID DID method in your Ceramic project.

## Design

The DID document for a 3ID is stored natively on Ceramic as a Tile Document StreamType, allowing for mutability and enabling it to securely handle key rotations. This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies.

See [CIP-79](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) for the full technical specification of the 3ID DID method.


## Other DID methods
Ceramic supports a few different DID methods for authentication. Below find links to the others:

- [Key DID](./key.md)
- NFT DID (coming soon)
- Safe DID (coming soon)

# 3ID DID

3ID (CIP-79) is a DID method supported by Ceramic which can be used to authenticate and perform [writes](). 3IDs are the most suitable DID method for end users, but can also be used for any subject such as orgs, businesses, apps, devices, etc.

## Usage
Developers can integrate and use 3ID for their application in 2 ways.

- Install a DID wallet that supports 3ID, such as [3ID Connect]()
- Install the low-level 3ID DID Provider

## Installation

### Install from npm

``` sh
$ npm install 3id-did-provider
```

###

#### Import the provider

``` javascript
import ThreeIdProvider from '3id-did-provider'
```

#### Get seed for DID

Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array.

??? info "How to generate a seed"
Here's how to securely generate a seed in the proper format:

``` javascript
import { randomBytes } from '@stablelib/random'
const seed = randomBytes(32)
```

#### Create a provider instance

##### Option 1: Using the seed

``` js
const threeId = await ThreeIdProvider.create({ getPermission, seed })
const provider = threeId.getDidProvider()

```

##### Option 2: Using an external auth method

This option is useful if you want to enable multiple secrets (seeds) that are capable of controlling the 3ID DID.

??? info "How to generate an authSecret"
Here's how to securely generate a seed in the proper format:

``` javascript
import { randomBytes } from '@stablelib/random'
const authSecret = randomBytes(32)
```

``` js
const authId = 'myAuthenticationMethod' // a name of the auth method

const threeId = await ThreeIdProvider.create({ getPermission, authSecret, authId })

const provider = threeId.getDidProvider()
```

??? info "Understanding `getPermission`"
The `getPermission` parameter is always required when creating an instance of ThreeIdProvider. It is used to give an application permission to decrypt and sign data. This function should present a dialog to the user in the wallet UI which asks for permission to access the given paths.

The function is called with one parameter which is the request object. It looks like this:

``` js
{
    type: 'authenticate',
    origin: 'https://my.app.origin',
    payload: {
        paths: ['/path/1', '/path/2']
    }
}
```

In the above example the app with origin `https://my.app.origin` is requesting access to /path/1 and /path/2. If the user approves, the function should return the paths array. If they decline, it will return an empty array. Note that a user may approve only some of the requested paths, which would return an array containing only the approved paths.

The most simple `getPermission` function simply grants all requested permissions.

``` javascript
const getPermission = async (request) => {
    return request.payload.paths
}
```






## **Tech Specs**
Below, find a simplified version of the 3ID specification. View the full [3ID (CIP-79) specification]().

### Method specfic identifier
`3`

Example:
```
did:3:<method-specific-identifier>
```

### DID Document
3ID offers a mutable DID document which can be used to set, remove, and rotate keys. This DID document is stored as a stream on Ceramic using the [TileDocument StreamType]().

### DID Resolver



When 3IDs are used in conjunction with IDX and the 3ID Keychain (as is implemented in 3ID Connect), a 3ID can easily be controlled with any number of blockchain accounts from any L1 or L2 network. This provides a way to unify a user's identity across all other platforms.



This includes the two default StreamTypes: Tile Documents and CAIP-10 Links.

3ID Connect is the most widely used authentication provider for Ceramic. The 3ID Connect authentication SDK allows users to use a 3ID DID with their existing blockchain wallet(s). 3ID Connect was designed for developers building Ceramic-enabled web apps whose users already have a blockchain/crypto wallet. By using 3ID Connect, your users get to rely on the signing and key management capabilities of the crypto wallet(s) they already have instead of needing to install a separate wallet. 3ID Connect is not a substitute for a crypto wallet. It works alongside these wallets.

Want to install 3ID Connect? Visit the installation section below.

Want to try 3ID Connect? Visit Self.ID to try it in a live application.

Supported blockchains¶
Ethereum
EVM compatible L1s and L2s (i.e. Avalanche, Moonbeam)
NEAR (coming soon)
EOS (coming soon)
Cosmos (coming soon)
Polkadot (coming soon)
Want to add a new blockchain? Follow this tutorial to make a PR.

Benefits¶
For users¶
Nothing to install: Users don't need to install 3ID Connect. They only need a blockchain wallet.
Same identity across accounts: Users can use the same DID across many different blockchain accounts.
Same identity across chains: Users can use the same DID across all L1 and L2 blockchain platforms.
No additional key management: Users don't need to manage the private key for their DID – just their existing blockchain wallet.
Automatic 3Box profile migration: Users' 3Box profiles will automatically be migrated by 3ID Connect the first time they use 3ID Connect via an application deployed on Ceramic Mainnet. Learn more about the 3Box profiles migration here.
For developers¶
Easy to install: See usage section below to add 3ID Connect to your project.
3ID provider: Access all methods in the 3ID DID provider.
Better user experience: User-friendly design makes it seamless for your users to use DIDs.
Works with web apps: Written in JavaScript/TypeScript and hosted in an iFrame.
Installation¶
Before installing 3ID Connect, you must have installed a Ceramic client and configured a DID for that client. By following the steps below, your users will be able to perform writes on Ceramic using a 3ID DID from their blockchain wallet.

1. Install from npm¶

npm install @3id/connect
2. Import the provider¶

import { ThreeIdConnect,  <BlockchainAuthProvider> } from '@3id/connect'

??? warning "Understanding `BlockchainAuthProvider`"
The `BlockchainAuthProvider` parameter is always required but the name shown here is just a placeholder. In your application, you should substitute in the specific BlockchainAuthProvider you are using. A full list of supported BlockchainAuthProviders can be found [here](https://github.com/ceramicnetwork/js-3id-blockchain-utils/tree/master/src/blockchains).
Example using an Ethereum wallet:


import { ThreeIdConnect,  EthereumAuthProvider } from '@3id/connect'
3. Request user's address¶

const addresses = await <blockchainName>.enable()
Example using an Ethereum wallet:


const addresses = await window.ethereum.enable()
4. Request authentication¶

const threeIdConnect = new ThreeIdConnect()
const authProvider = new <BlockchainAuthProvider>(<blockchainName>, addresses[0])
await threeIdConnect.connect(authProvider)
Example using an Ethereum wallet:


const authProvider = new EthereumAuthProvider(window.ethereum, addresses[0])
await threeIdConnect.connect(authProvider)

  !!! warning ""
This will prompt the user with a 3ID Connect permissions window.
5. Create provider instance¶

const provider = await threeIdConnect.getDidProvider()
6. Set the provider¶
Set the Provider instance to the DID instance used by your Ceramic client in order to perform writes.


ceramic.did.setProvider(provider)
7. Authenticate the DID¶

await ceramic.did.authenticate()

  !!! warning ""
This will prompt the user with a 3ID Connect permissions window.
Your users will now be authenticated and can perform writes to streams on Ceramic.

Underlying technologies¶
3ID Connect is built on open source technologies and standards. Learn more about the technologies that make 3ID Connect possible.

3ID DIDs (CIP-79): For creating decentralized identifiers and authenticating to Ceramic
IDX (CIP-11): Protocol and SDK for storing data streams controlled by the user's 3ID
3ID Keychain (CIP-20): IDX definition and schema for using a TileDocument StreamType to store a list of encrypted authentication secrets for each wallet account
CAIP10Link (CIP-7): StreamType for publicly linking a wallet account to a 3ID DID
Crypto accounts (CIP-21): IDX definition and schema for using a TileDocument StreamType to store a list of streamIDs for a DID's CAIP10Links
Maintainers¶
3ID Connect is maintained by 3Box Labs.

License¶
3ID Connect is 100% open source under MIT and Apache 2.
