# Authentication
How to set up authentication for Ceramic.

??? tip "When to use authentication"
    Authentication is needed when you want to [write to the network](transactions.md). If you only want to interact with Ceramic in a read-only manner, then you can simply [query the network](queries.md) without authentication.

## Prerequisites

Authentication requires having [installed Ceramic](clients.md) in your application. 

## Choose your setup

### DID method

The first step in adding authentication to your application is choosing which [DID method](../learn/dids/methods.md) you want to support for user accounts.

DID Method | Description | Registration | DID Documents | Details |
| ------ | ----- | ---- | ----- | ----- |
| 3ID DID | A complete, flexible DID method built on Ceramic that supports key rotations and revocations | Ceramic | Mutable | [Learn]() |
| Key DID | A lightweight, inflexible DID method that does not support key rotations | None | Immutable | [Learn]() |

### DID provider or wallet

After deciding on a DID method, you need to install either a [provider](../learn/dids/providers.md) or [wallet](../learn/dids/wallets.md) for that method. The most commonly used DID providers and wallets can be found below.

| Name      | DID Method | Description | Details |
| ----------- | ------ | ---- | ----- |
| `key-did-provider-ed25519` | Key DID | A software library for creating and interacting with Key DIDs. | [Learn]() |
| `3id-did-provider` | 3ID DID | A software library for creating and interacting with 3ID DIDs. | [Learn]() |
| 3ID Connect | 3ID DID | A hosted wallet and authentication system for browser apps using the 3ID DID method. Allows users to authenticate with their existing blockchain wallets. | [Learn]() |

!!! tip ""
    When using a provider, applications **are** responsible for key management and security. When using a wallet, applications are **not** responsible for key management since it is handled by the wallet. We recommend using a wallet if possible.

## Installation
Install the DID provider or wallet in your project.

=== "key-did-provider"

    ``` sh
    $ npm install key-did-provider-ed25519
    ```

=== "3id-did-provider"

    ``` sh
    $ npm install 3id-did-provider
    ```

=== "3ID Connect"

    ```sh
    $ npm install @ceramicstudio/3id-connect
    ```
    
## Authentication

The authentication process varies depending on which provider or wallet you are using. Closely follow the steps below.

=== "key-did-provider"

    #### Import the provider

    Import the Key DID provider into your project.

    ``` javascript
    import { Ed25519Provider } from 'key-did-provider-ed25519'
    ```

    #### Get seed for DID

    Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array.

    ??? note "How to generate a seed"
        Here's how to securely generate a seed in the proper format:

        ``` javascript
        import { randomBytes } from '@stablelib/random'
        const seed = randomBytes(32)
        ```

    #### Create a provider instance using the seed

    ``` js
    const provider = new Ed25519Provider(seed)
    ```

=== "3id-did-provider"

    #### Import the provider

    ``` javascript
    import ThreeIdProvider from '3id-did-provider'
    ```

    #### Get seed for DID

    Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array.

    ??? note "How to generate a seed"
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

    ??? note "How to generate an authSecret"
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

    ??? note "Understanding the `getPermission` function"
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

        The most simple `getPermission` function simply grants all requested permissions (example below).`

        ``` javascript
        const getPermission = async (request) => {
            return request.payload.paths
        }
        ```


=== "3ID Connect"

    #### Import the provider

    ``` javascript
    import { ThreeIdConnect,  <BlockchainAuthProvider> } from '@ceramicstudio/3id-connect'
    ```

    Example using an Ethereum wallet:

    ``` javascript
    import { ThreeIdConnect,  EthereumAuthProvider } from '@ceramicstudio/3id-connect'
    ```

    ??? note "Understanding the `BlockchainAuthProvider`"
        The `BlockchainAuthProvider` parameter is always required but the name shown here is just a placeholder. In your application, you should substitute in the specific BlockchainAuthProvider you are using. A full list of BlockchainAuthProviders can be found [here]().

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

## Set the provider

Set the authenticated provider instance to your Ceramic client in order to perform transactions.
``` javascript
await ceramic.setDIDProvider(provider)
```
    

## Usage

After authenticating, the user will now be able to perform [transactions]() on Ceramic using their DID.

</br>
</br>
</br>