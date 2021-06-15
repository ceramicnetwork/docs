# 3ID DID Provider
3ID DID Provider is a low-level JavaScript library that provides interfaces for creating and authenticating 3IDs. For most applications seeking a more complete and user-friendly 3ID DID Provider, it is recommended that you instead use a higher-level library, such as [3ID Connect](./3id-connect.md).

## **Installation**
Before installing 3ID DID Provider, you must have [installed a Ceramic client](../../build/installation.md). By following the steps below, users will be able to [perform writes](../../build/writes.md) on Ceramic using a 3ID DID.

### 1. Install from npm

``` sh
npm install 3id-did-provider
```

### 2. Import the 3ID Provider

``` javascript
import ThreeIdProvider from '3id-did-provider'
```

### 3. Get a seed phrase

Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array. Here's how to securely generate a `seed` in the proper format:

``` javascript
import { randomBytes } from '@stablelib/random'
const seed = randomBytes(32)
```

### 4. Create a 3ID Provider instance

#### Option 1: Using the seed

``` js
const threeId = await ThreeIdProvider.create({ getPermission, seed })
const provider = threeId.getDidProvider()

```

#### Option 2: Using an external auth method

This option enables one or more secrets (seeds) to control the 3ID DID. It is useful, for example, if you want to control a 3ID DID using one or more blockchain wallets. This flow is what is implemented by [3ID Connect](./3id-connect.md).

``` js
const authId = 'myAuthenticationMethod' // a name of the auth method

const threeId = await ThreeIdProvider.create({ getPermission, authSecret, authId })

const provider = threeId.getDidProvider()
```

How to securely generate an `authSecret` in the proper format:

``` javascript
import { randomBytes } from '@stablelib/random'
const authSecret = randomBytes(32)
```

How to use the `getPermission` parameter:

The `getPermission` parameter is always required when creating an instance of ThreeIdProvider. It allows your application to request actions from a 3ID, such as signing and/or decrypting data. When called, this function should prompt the user in the UI asking for permission to the given paths. You would likely need to implement a UI for this.

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

### 5. Set the instance to Ceramic

Set the 3ID DID Provider instance to the DID instance used by your Ceramic client.

``` javascript
ceramic.did.setProvider(provider)
```

### 6. Authenticate the 3ID
Now all that's left is to authenticate to the Ceramic client's DID instance using the configured 3ID DID Provider.

``` js
await ceramic.did.authenticate()
```

## **Next steps: Writes**

After authenticating with the 3ID DID Provider, users will now be able to perform [writes](writes.md).

## License
3ID DID Provider is fully open sourced under MIT and Apache 2.

</br></br></br>
