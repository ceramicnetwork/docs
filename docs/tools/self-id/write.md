# **Self.ID Web**

The `@self.id/web` module provides user authentication, data storage, and retrieval for browser-based web applications.

> If you're building with React, we recommend using [Self.ID Framework]() instead.

## **Getting started with Self.ID Web**

---

Visit the [**Self.ID Web reference →**](../../reference/self-id/classes/web.SelfID.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with Self.ID Web:

### **Installation**
Install `@self.id/web` from npm:

``` bash
npm install @self.id/web
```

### **User authentication**

The Self.ID web module exports the `SelfID` and `WebClient` classes. Either can be used for user authentication via an Ethereum or EVM-comppatible wallet:

#### Using the SelfID class

The process of creating all instances needed by your web application (`WebClient`, `DID` and `SelfID`) can be reduced to using the [`SelfID.authenticate()`](../../reference/self-id/classes/web.SelfID.md#authenticate) static authentication method:

```ts
import { EthereumAuthProvider, SelfID } from '@self.id/web'

// The following assumes there is an injected `window.ethereum` provider
const addresses = await window.ethereum.request({
  method: 'eth_requestAccounts',
})

// The following configuration assumes your local node is connected to the Clay testnet
const self = await SelfID.authenticate({
  authProvider: new EthereumAuthProvider(window.ethereum, addresses[0]),
  ceramic: 'local',
  connectNetwork: 'testnet-clay',
})
```

#### Using the WebClient class

The [`WebClient`](../../reference/self-id/classes/web.WebClient.md) class extends `Core` from the `@self.id/core` package with the additional `connectNetwork` parameter to specify the Ceramic network that should be used by 3ID Connect:

```ts
import { EthereumAuthProvider, SelfID, WebClient } from '@self.id/web'

// The following assumes there is an injected `window.ethereum` provider
const addresses = await window.ethereum.request({
  method: 'eth_requestAccounts',
})
const authProvider = new EthereumAuthProvider(window.ethereum, addresses[0])

// The following configuration assumes your local node is connected to the Clay testnet
const client = new WebClient({
  ceramic: 'local',
  connectNetwork: 'testnet-clay',
})

// If authentication is successful, a DID instance is attached to the Ceramic instance
await client.authenticate(authProvider)

// A SelfID instance can only be created with an authenticated Ceramic instance
const self = new SelfID({ client })
```

### **Data management**

After authenticating the user with either of the above methods, your application can perform data storage and retrieval interactions with the user based on a data model (definition):

```ts
await self.set('basicProfile', { name: 'Alice' })
```
