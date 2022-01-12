# Authentication and write access

Performing writes (creating and updating streams) in Ceramic requires an authenticated DID.

The Self.ID SDK leverages [3ID Connect](../../reference/accounts/3id-did.md#3id-connect) to provide browser-based authentication in the `@self.id/web` package via the [`EthereumAuthProvider`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html).

## Using the WebClient class

The `WebClient` class extends `Core` from the `@self.id/core` package with the additional `connectNetwork` parameter to specify the Ceramic network that should be used by 3ID Connect:

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

await self.set('basicProfile', { name: 'Alice' })
```

[WebClient API reference](../../reference/self-id/classes/web.WebClient.md){: .md-button .md-button }

## Using the SelfID class directly

The process of creating a `WebClient`, `DID` and `SelfID` instances can be reduced to using the [`SelfID.authenticate()`](../../reference/self-id/classes/web.SelfID.md#authenticate) static method:

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

await self.set('basicProfile', { name: 'Alice' })
```

[SelfID API reference](../../reference/self-id/classes/web.SelfID.md){: .md-button .md-button }
