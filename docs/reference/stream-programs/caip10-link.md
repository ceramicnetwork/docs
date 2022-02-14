# **CAIP-10 Link client**

---

A CAIP-10 Link is a stream that stores a proof that links a blockchain address to a Ceramic account (DID), using the [CAIP-10 standard](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) to represent blockchain addresses.

The `stream-caip10-link` module export a `Caip10Link` class used to link and unlink a DID to a CAIP-10 address using the [CIP-7 "CAIP-10 Link" program](../../docs/advanced/standards/stream-programs/caip10-link.md).

## **Installation**

---

```sh
npm install @ceramicnetwork/stream-caip10-link
```

### **Additional requirements**

- In order to load CAIP-10 Links, a [Ceramic client instance](../core-clients/ceramic-http.md) must be available
- To add/remove links, the client must also have an [authenticated DID](../core-clients/did-jsonrpc.md)
- An authentication provider is needed to sign the payload for the given CAIP-10 account, using the `blockchain-utils-linking` module that should be installed as needed:

```sh
npm install @ceramicnetwork/blockchain-utils-linking
```

## **Common usage**

---

### **Load a link**

In this example we load a Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`).

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'

const ceramic = new CeramicClient()

async function getLinkedDID() {
  // Using the Ceramic client instance, we can load the link for a given CAIP-10 account
  const link = await Caip10Link.fromAccount(
    ceramic,
    'eip155:1:0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8',
  )
  // The `did` property of the loaded link will contain the DID string value if set
  return link.did
}
```

### **Create a link**

Here we can see the full flow of getting a user's Ethereum address, creating a link, and adding the users' DID account.

In this example we create a Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`) and then associate it with the DID `did:3:k2t6...ydki`.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'
import { EthereumAuthProvider } from '@ceramicnetwork/blockchain-utils-linking'

const ceramic = new CeramicClient()

async function linkCurrentAddress() {
  // First, we need to create an EthereumAuthProvider with the account currently selected
  // The following assumes there is an injected `window.ethereum` provider
  const addresses = await window.ethereum.request({
    method: 'eth_requestAccounts',
  })
  const authProvider = new EthereumAuthProvider(window.ethereum, addresses[0])

  // Retrieve the CAIP-10 account from the EthereumAuthProvider instance
  const accountId = await authProvider.accountId()

  // Load the account link based on the account ID
  const accountLink = await Caip10Link.fromAccount(
    ceramic,
    accountId.toString(),
  )

  // Finally, link the DID to the account using the EthereumAuthProvider instance
  await accountLink.setDid(
    'did:3:k2t6wyfsu4pg0t2n4j8ms3s33xsgqjhtto04mvq8w5a2v5xo48idyz38l7ydki',
    authProvider,
  )
}
```

### **Remove a link**

Removing a link involves a similar flow to setting the DID, but using the `clearDid` method instead of `setDid`:

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'
import { EthereumAuthProvider } from '@ceramicnetwork/blockchain-utils-linking'

const ceramic = new CeramicClient()

async function unlinkCurrentAddress() {
  // First, we need to create an EthereumAuthProvider with the account currently selected
  // The following assumes there is an injected `window.ethereum` provider
  const addresses = await window.ethereum.request({
    method: 'eth_requestAccounts',
  })
  const authProvider = new EthereumAuthProvider(window.ethereum, addresses[0])

  // Retrieve the CAIP-10 account from the EthereumAuthProvider instance
  const accountId = await authProvider.accountId()

  // Load the account link based on the account ID
  const accountLink = await Caip10Link.fromAccount(
    ceramic,
    accountId.toString(),
  )

  // Finally, unlink the DID from the account using the EthereumAuthProvider instance
  await accountLink.clearDid(authProvider)
}
```

<!--
## Additional Resources

- [CIP-10: CAIP10 Link Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md)
- [Complete CAIP10Link.js API Reference]()

## Next Steps

---

- [Next step 1]()
-->
