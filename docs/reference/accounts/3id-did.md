# **3ID DID libraries**

---

The 3ID DID libraries provide support for the `did:3` method as a Ceramic account, which notably uses Ceramic to store DID documents as Ceramic streams, enabling features such as supporting multiple authentication keys and rotating these keys.

## **Available libraries**

---

- The [3ID DID resolver](#3id-did-resolver) allows a DID JSON-RPC client to resolve accounts using the `did:3` method from the Ceramic network
- The [3ID DID provider](#3id-did-provider) allows applications to create and use 3ID DID accounts to perform transactions
- [3ID Connect](#3id-connect) exposes a hosted implementation of the 3ID DID provider for use with web applications (recommended)

## **3ID DID resolver**

---

A 3ID DID resolver is needed to resolve DID documents using the `did:3` method.

### **Installation**

```sh
npm install @ceramicnetwork/3id-did-resolver key-did-resolver
```

A Ceramic client is also needed to use this resolver:

```sh
npm install @ceramicnetwork/http-client
```

### **Usage**

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DID } from 'dids'
import { getResolver as getKeyResolver } from 'key-did-resolver'
import { getResolver as get3IDResolver } from '@ceramicnetwork/3id-did-resolver'

function createCeramicWith3ID() {
  const ceramic = new CeramicClient()
  const did = new DID({
    resolver: {
      // A Ceramic client instance is needed by the 3ID DID resolver to load DID documents
      ...get3IDResolver(ceramic),
      // `did:key` DIDs are used internally by 3ID DIDs, therefore the DID instance must be able to resolve them
      ...getKeyResolver(),
    },
  })
  // This will allow the Ceramic client instance to resolve DIDs using the `did:3` method
  ceramic.did = did
}
```

## **3ID DID provider**

---

The 3ID DID provider module implements a provider to create and manage DIDs using the `did:3` method, either using a `seed` or an `authId` associated to an `authSecret` (recommended).

It is up to applications using the 3ID DID provider to take care of the security of authentication secrets and seeds, whether they are providing a custodial key management, prompting users to input secrets, or alternative solutions.

### **Installation**

```sh
npm install @3id/did-provider
```

### **Permissions management**

The `getPermission` parameter is always required when creating an instance of `ThreeIdProvider`. It allows your application to request actions from a 3ID, such as signing and/or decrypting data. When called, this function should prompt the user in the UI asking for permission to the given paths. You would likely need to implement a UI for this.

The function is called with one parameter which is the request object. It looks like this:

```js
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

```js
function getPermission(request) {
  return Promise.resolve(request.payload.paths)
}
```

### **Usage with authID and secret**

This is the **recommended method** for using a 3ID DID provider.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DID } from 'dids'
import { getResolver as getKeyResolver } from 'key-did-resolver'
import { getResolver as get3IDResolver } from '@ceramicnetwork/3id-did-resolver'
import { ThreeIdProvider } from '@3id/did-provider'

// `authSecret` must be a 32-byte long Uint8Array
async function authenticateWithSecret(authSecret) {
  const ceramic = new CeramicClient()

  const threeID = await ThreeIdProvider.create({
    authId: 'myAuthID',
    authSecret,
    // See the section above about permissions management
    getPermission: (request) => Promise.resolve(request.payload.paths),
  })

  const did = new DID({
    provider: threeID.getDidProvider(),
    resolver: {
      ...get3IDResolver(ceramic),
      ...getKeyResolver(),
    },
  })

  // Authenticate the DID using the 3ID provider
  await did.authenticate()

  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}
```

### **Usage with seed**

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DID } from 'dids'
import { getResolver as getKeyResolver } from 'key-did-resolver'
import { getResolver as get3IDResolver } from '@ceramicnetwork/3id-did-resolver'
import { ThreeIdProvider } from '@3id/did-provider'

// `seed` must be a 32-byte long Uint8Array
async function authenticateWithSecret(seed) {
  const ceramic = new CeramicClient()

  const threeID = await ThreeIdProvider.create({
    seed,
    // See the section above about permissions management
    getPermission: (request) => Promise.resolve(request.payload.paths),
  })

  const did = new DID({
    provider: threeID.getDidProvider(),
    resolver: {
      ...get3IDResolver(ceramic),
      ...getKeyResolver(),
    },
  })

  // Authenticate the DID using the 3ID provider
  await did.authenticate()

  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}
```

## **3ID Connect**

---

3ID Connect is an in-browser [3ID DID provider](#3id-did-provider), using blockchain wallets to create deterministic authentication secrets using to control a DID.

Using 3ID Connect, web apps do not need to take care of key custody directly, but rather to use an authentication provider such as `EthereumAuthProvider` to allow 3ID Connect to generate the necessarry authentication secrets.

### **Installation**

```sh
npm install @3id/connect
```

### **Usage**

> 3ID Connect is a **browser-only** library, if your app has shared code between browser and server logic, you need to make sure the following code is not executed server-side, otherwise it will not work and may throw errors.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DID } from 'dids'
import { getResolver as getKeyResolver } from 'key-did-resolver'
import { getResolver as get3IDResolver } from '@ceramicnetwork/3id-did-resolver'
import { EthereumAuthProvider, ThreeIdConnect } from '@3id/connect'

// Create a ThreeIdConnect connect instance as soon as possible in your app to start loading assets
const threeID = new ThreeIdConnect()

async function authenticateWithEthereum(ethereumProvider) {
  // Request accounts from the Ethereum provider
  const accounts = await ethereumProvider.request({
    method: 'eth_requestAccounts',
  })
  // Create an EthereumAuthProvider using the Ethereum provider and requested account
  const authProvider = new EthereumAuthProvider(ethereumProvider, accounts[0])
  // Connect the created EthereumAuthProvider to the 3ID Connect instance so it can be used to
  // generate the authentication secret
  await threeID.connect(authProvider)

  const ceramic = new CeramicClient()
  const did = new DID({
    // Get the DID provider from the 3ID Connect instance
    provider: threeID.getDidProvider(),
    resolver: {
      ...get3IDResolver(ceramic),
      ...getKeyResolver(),
    },
  })

  // Authenticate the DID using the 3ID provider from 3ID Connect, this will trigger the
  // authentication flow using 3ID Connect and the Ethereum provider
  await did.authenticate()

  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}

// When using extensions such as MetaMask, an Ethereum provider may be injected as `window.ethereum`
async function tryAuthenticate() {
  if (window.ethereum == null) {
    throw new Error('No injected Ethereum provider')
  }
  await authenticateWithEthereum(window.ethereum)
}
```
