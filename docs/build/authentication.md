# Authentication
This guide will help you add user authentication to your project. Authentication is needed when you want to perform [writes](writes.md). If you only want to perform [queries](queries.md), you do not need authentication.

## Prerequisites

Authentication requires having [installed a Ceramic client](installation.md) in your project.

## Choose your setup

### DID method

The first step in adding authentication to your project is choosing which
*DID method* you want to support for user accounts. Due to their mutability and security, it is recommended that you use 3ID DID for users.
<!--[DID method](../learn/dids/methods.md) you want to support for user accounts.-->

DID Method | Description | Registration | DID Documents | Details |
| ------ | ----- | ---- | ----- | ----- |
| 3ID DID | A complete and flexible DID method built on Ceramic that supports key rotations and revocations | Ceramic | Mutable | [Learn](https://github.com/ceramicstudio/js-3id-did-provider){:target="_blank"} |
| Key DID | A lightweight but inflexible DID method that does not support key rotations | None | Immutable | [Learn](https://github.com/ceramicnetwork/key-did-provider-ed25519){:target="_blank"} |

### DID provider or wallet

After deciding on a DID method, you need to install either a *wallet* or a *provider* for that method. The most commonly used DID providers and wallets can be found below. For most browser applications, it is recommended that you use 3ID Connect.

| Name      | DID Method | Type | Description | Details |
| ----------- | ------ | ---- | ----- | --- |
| 3ID Connect | 3ID DID | Wallet | A hosted wallet and authentication system for browser apps using 3ID DIDs. Your application is not responsible for key management, and users can authenticate with their existing blockchain wallets. | [Learn](https://github.com/ceramicstudio/3id-connect){:target="_blank"} |
| `3id-did-provider` | 3ID DID | Provider | A JavaScript library for creating and interacting with 3ID DIDs. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret. | [Learn](https://github.com/ceramicstudio/js-3id-did-provider){:target="_blank"} |
| `key-did-provider-ed25519` | Key DID | Provider | A JavaScript library for creating and interacting with Key DIDs. Your application is responsible for key managemet, and users need to authenticate with a DID seed. | [Learn](https://github.com/ceramicnetwork/key-did-provider-ed25519){:target="_blank"} |

## Installation

Install a DID wallet or provider in your project using npm.

=== "3ID Connect"

    ```sh
    $ npm install 3id-connect@next
    ```
    
=== "3id-did-provider"

    ``` sh
    $ npm install 3id-did-provider
    ```
    
=== "key-did-provider"

    ``` sh
    $ npm install key-did-provider-ed25519
    ```


## Authentication

The authentication process varies depending on which wallet or provider you are using. Closely follow the steps below.

=== "3ID Connect"

    #### Import the provider

    ``` javascript
    import { ThreeIdConnect,  <BlockchainAuthProvider> } from '3id-connect'
    ```

    Example using an Ethereum wallet:

    ``` javascript
    import { ThreeIdConnect,  EthereumAuthProvider } from '3id-connect'
    ```

    ??? info "Understanding `BlockchainAuthProvider`"
        The `BlockchainAuthProvider` parameter is always required but the name shown here is just a placeholder. In your application, you should substitute in the specific BlockchainAuthProvider you are using. A full list of supported BlockchainAuthProviders can be found [here](https://github.com/ceramicnetwork/js-3id-blockchain-utils/tree/master/src/blockchains).

    #### Request the user's blockchain address

    ``` js
    const addresses = await <blockchainName>.enable()
    ```

    Example using an Ethereum wallet:

    ``` js
    const addresses = await window.ethereum.enable()
    ```

    #### Request authentication from the user's blockchain wallet
    This will prompt the user with a 3ID Connect permissions window.

    ``` js
    const authProvider = new <BlockchainAuthProvider>(<blockchainName>, addresses[0])
    await threeIdConnect.connect(authProvider)
    ```

    Example using an Ethereum wallet:

    ``` js
    const authProvider = new EthereumAuthProvider(window.ethereum, addresses[0])
    await threeIdConnect.connect(authProvider)
    ```

    #### Create a provider instance

    ``` js
    const provider = await threeIdConnect.getDidProvider()
    ```

=== "3id-did-provider"

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

=== "key-did-provider"

    #### Import the provider

    Import the Key DID provider into your project.

    ``` javascript
    import { Ed25519Provider } from 'key-did-provider-ed25519'
    ```

    #### Get seed for DID

    Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array.

    ??? info "How to generate a seed"
        Here's how to securely generate a seed in the proper format:

        ``` javascript
        import { randomBytes } from '@stablelib/random'
        const seed = randomBytes(32)
        ```

    #### Create a provider instance using the seed

    ``` js
    const provider = new Ed25519Provider(seed)
    ```

## Set the provider

Set the authenticated provider instance to your Ceramic client in order to perform writes.
``` javascript
await ceramic.setDIDProvider(provider)
```

## Usage

After authenticating, the user will now be able to perform [writes](writes.md) on Ceramic using their DID.

</br>
</br>
</br>
