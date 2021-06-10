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
  

## Responsibilities

- 
- Authenticate to Ceramic
- Approve application permissions



## Technology

[3ID DID](../dids/3id.md)




## Responsibilities



## **How it works**

### Users without a 3ID linked to their wallet account

1. User arrives at your application and signs in with their blockchain wallet
2. User sees a 3ID Connect account creation prompt asking if they want to create a new 3ID for this account or link to an existing 3ID
3. If user selects "Create New 3ID", they will be prompted in the wallet to sign two messages: one to add this account as an authentication method for their 3ID, and another to create a CAIP-10 Link which publicly associated their account with their 3ID. If the user selects "Link to Existing 3ID", they will be directed to a page where they can select a 3ID from a list of known 3IDs, then they will need to approve the two messages mentioned previously.

### Users with a 3ID linked to their wallet account

1. User arrives at your application and signs in with their blockchain wallet
2. User sees a 3ID Connect permissions prompt asking them to grant your application permissions
3. Once approved, user is authenticated

On future logins, users will not see the permissions prompt again unless they clear local storage for 3ID Connect.

## **Design**

Under the hood 3ID Connect is built on a set of open standards:

- 3ID DID Method
- IDX
- 3ID Keychain
- CAIP-10 Links 

## Adding new blockchains

Follow this [**tutorial**]() to add support for new blockchains to 3ID Connect.

</br>
</br>
</br>
