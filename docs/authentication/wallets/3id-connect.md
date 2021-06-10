# 3ID Connect

3ID Connect is a javascript SDK that allows users to authenticate to your Ceramic app with their existing blockchain wallets. 3ID Connect is the most widely used 3ID DID provider. When using 3ID Connect, your application is not responsible for key management.

#### Supported blockchains
- Ethereum
- Avalanche
- Other EVM compatible L1s and L2s
- NEAR
- Moonbeam
- EOS
- Cosmos (coming soon)
- Polkadot (coming soon)

> Want to add a new blockchain? Follow this [**tutorial**](https://blog.ceramic.network/add-authentication-with-new-blockchains-in-3id-connect/).


## **User flows**
You can try the 3ID Connect flows by using [Self.ID](https://self.id).

### New account
This is the flow experienced by a user the first time they use a given blockchain account with 3ID Connect, on *any* application.

1. User arrives at your site
2. User connects with their blockchain wallet <insert image>
3. User sees an `identity creation` prompt asking if they want to "Create a new 3ID" or "Use an existing 3ID" <insert image>

    3a. If user selects "Create new 3ID", they will be prompted in the wallet to sign two messages: one to add this account as an authentication method, and another to publicly associated the account with the 3ID <insert image>

    3b. If the user selects "Use an existing 3ID", they will be directed to a page where they can select a 3ID from a list of known 3IDs, then they will need to approve the two messages mentioned previously. 

### Returning account
This is the flow experiences by a user every other time they use a given blockchain account with 3ID Connect, on *any* application – i.e. the user's blockchain account is already associated with a 3ID.

1. User arrives at your site
2. User connects with their blockchain wallet
3. User sees a `permission request` from your application <insert image>
4. Once approved, user is authenticated

> On future logins, users will not see the permissions prompt again unless they clear local storage for 3ID Connect.

## **Usage**

Before installing 3ID Connect, you must have [installed a Ceramic client](installation.md) and [configured a DID](configure-did.md) for that client. By following the steps below, your users will be able to perform writes on Ceramic using their blockchain wallet.


### 1. Installation

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

### 6. Set the provider
Set the Provider instance to the DID instance used by your Ceramic client in order to perform writes.

```
ceramic.did.setProvider(provider)
```

### 7. Authenticate the DID

```
await ceramic.did.authenticate()
```

      !!! warning ""
    This will prompt the user with a 3ID Connect permissions window.
  
## Advanced topics

### Initializing a DID
    
### Key management
3ID takes care of it for you, and wallets take care of it for us. Describe the key management model in detail.

### 3Box DID migration
3ID Connect handles the migration of DIDs from the 3Box system to Ceramic and IDX.


## Underlying technologies

3ID Connect is built on a set of open standards. Learn more about the technologies that make 3ID Connect possible.

- [3ID DIDs](../dids/3id.md): For creating decentralized identifiers and authenticating to Ceramic
- [IDX](../../tools/identity/idx.md): Protocol and SDK for storing data streams controlled by the user's [3ID]()
- [3ID Keychain](): IDX [definition]() for storing encrypted authentication secrets for each wallet account
- [Crypto accounts](): IDX [definition]() for storing encrypted authentication secrets for each wallet account
- [CAIP10Link](): StreamType for publicly linking a wallet account to a 3ID DID
    
## Maintainers
3Box Labs are the maintainers of 3ID Connect. 3ID Connect is 100% open source under MIT and Apache 2.

</br>
</br>
</br>
