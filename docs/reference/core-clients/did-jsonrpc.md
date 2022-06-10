# **DID JSON-RPC client**

---

DID JSON-RPC client provides a simple JS API for interacting with Ceramic accounts.

## **Things to know**

---

- Provides the DID object, which must be authenticated, then mounted on the Ceramic object to perform transactions.
- For Ceramic nodes, the DID client acts as a way to resolve and verify transaction signatures
- For Ceramic clients, the DID client serves as a way to create an account, authenticate, sign, encrypt
- If your project requires transactions, you **need** to install this package or one that offers similar EIP-2844 API support.
- The DID client library can be used in both browser and Node.js environments.
- It supports any DID wallet provider that adheres to the [EIP-2844](https://eips.ethereum.org/EIPS/eip-2844) interface.
- Communicating between a Ceramic client and any account provider.
- Ceramic does not work without a DID client, as it is how all participants are identified and how transactions and messages are signed and verified.

## Installation

```sh
npm install dids
```

The `DID` class provides the interface on top of underlying account libraries. The next step is to set up your account system, which requires you to make some important decisions about your account model and approach to key management. This process consists of three steps: choosing your account types, installing a provider, and installing resolver(s).

## Choose your account types

Choosing an account type can significantly impact your users' identity and data interoperability.  For example, some account types are fixed to a single public key (Key DID, PHK DID), so the data is siloed to that key.  In contrast, others (3ID DID) have mutable key management schemes that can support multiple authorized signing keys and works cross-chain with blockchain wallets. Visit each account to learn more about its capabilities.

### [3ID DID](../../docs/advanced/standards/accounts/3id-did.md)

Supports multi-account, cross-chain (MACC) identities. Mutability and key rotations. Good for users + most popular. Relies on Ceramic for account storage.

### [Key DID](../../docs/advanced/standards/accounts/key-did.md)

Simple, self-contained DID method.

### [NFT DID](../../docs/advanced/standards/accounts/nft-did.md)

**Experimental** DID method using NFT ownership for authentication.

### [Safe DID](../../docs/advanced/standards/accounts/safe-did.md)

**Experimental** DID method using a Gnosis Safe for authentication.

<!--
<div class="txtl-options half">
  <a href="http://127.0.0.1:8000/authentication/3id-did/method/" class="box">
    <h5>3ID DID →</h5>
    <p>Supports multi-account, cross-chain (MACC) identities. Mutability and key rotations. Good for users + most popular. Relies on Ceramic for account storage.</p>
  </a>
  <span class="box-space"> </span>
  <a href="./hub/apis" class="box">
    <h5>PKH DID →</h5>
    <p>Launch a node to provide storage, compute, and retrieval to your app or other apps on the network.</p>
  </a>
</div>
<div class="txtl-options half">
  <a href="./tutorials/hub/web-app/" class="box">
    <h5>Key DID →</h5>
    <p>Good for developers.</p>
  </a>
  <span class="box-space"> </span>
  <a href="./hub/" class="box">
    <h5>NFT DID →</h5>
    <p>Easily develop web applications with a single SDK that includes everything you need.</p>
  </a>
</div>
<div class="txtl-options half">
  <a href="./tutorials/hub/web-app/" class="box">
    <h5>Safe DID →</h5>
    <p>Connect to Ceramic with your preferred programming language.</p>
  </a>
</div>
-->

## Install account resolvers

The next step is to install resolver libraries for all account types that you may need to read and verify data (signatures). This includes _at least_ the resolver for the provider or wallet chosen in the previous step. However, most projects install all resolvers to be safe:

| Account | Resolver libraries                                                            | Maintainer |
| ------- | ----------------------------------------------------------------------------- | ---------- |
| 3ID DID | [`@ceramicnetwork/3id-did-resolver`](../accounts/3id-did.md#3id-did-resolver) | 3Box Labs  |
| Key DID | [`key-did-resolver`](../accounts/key-did.md#key-did-resolver)                 | 3Box Labs  |

<!--
| PKH DID  | [`js-pkh-did-resolver →`]() | Spruce             |
| NFT DID  | [`3id/did-resolver →`]()    | @someonehandle.xyz |
| Safe DID | [`3id/did-resolver →`]()    | 3Box Labs          |
-->

## Install account providers

Install providers to manage accounts and sign transactions. Once you have chosen one or more account types, you'll need to install the providers for these account types. These will enable the client-side creation and use of accounts within your application. If your application uses Ceramic in a read-only manner without transactions, you do not need to install a provider.

### Using web wallets

However, the providers listed above are low-level, run locally, and burden developers with UX issues related to secret key management and transaction signing. Instead of using a local provider, you can alternatively use a wallet system. Wallets wrap providers with additional user experience features related to signing and key management and can be used in place of a provider. The benefit is multiple applications can access the same wallet and key management system, so users have a continuous experience between applications.

| Account | Wallet                                              | Benefits                                                                                                                                                                                 | Drawbacks |
| ------- | --------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- |
| 3ID DID | [`3ID Connect`](../accounts/3id-did.md#3id-connect) | ✅ Sign with blockchain wallets, including MetaMask</br>✅ Connect multiple blockchain accounts to one Ceramic account</br>✅ Implements 3ID DID</br>✅ Works cross-chain</br>✅ Sleek UI | ❌ Risks  |

> Most user-facing applications use the 3ID Connect wallet instead of using a provider.

### Create your own wallet

One option is installing and setting up one or more account providers that run locally. Note that these local signers have different wallet support

| Account | Supported Key Types | Provider libraries                                               |
| ------- | ------------------- | ---------------------------------------------------------------- |
| 3ID DID | Ed25519             | [`3id-did-provider`](../accounts/3id-did.md#3id-did-provider)    |
| Key DID | Ed25519             | [`key-did-provider-ed25519`](../accounts/key-did.md#ed25519)     |
| Key DID | Secp256k1           | [`key-did-provider-secp256k1`](../accounts/key-did.md#secp256k1) |

<!-- | PKH DID | ?????????           | [`js-pkh-did-provider →`]()           | -->

Note that NFT DID and Safe DID do not have a signer because they are compatible with all other providers.

## Setup your project

You should have installed DID.js and set up your account system, including authentication to perform transactions. When you include everything in your project, it should look like this. Note that the exact code will vary by your setup, including provider and wallet. Consult your provider's documentation for authentication specifics.

```ts
// Import DID client
import { DID } from 'dids'

// Add account system
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

// Connect to a Ceramic node
const API_URL = 'https://your-ceramic-node.com'

// Create the Ceramic object
const ceramic = new CeramicClient(API_URL)

// ↑ With this setup, you can perform read-only queries.
// ↓ Continue to authenticate the account and perform transactions.

async function authenticateCeramic(seed) {
  // Activate the account by somehow getting its seed.
  // See further down this page for more details on
  // seed format, generation, and key management.
  const provider = new Ed25519Provider(seed)
  // Create the DID object
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate with the provider
  await did.authenticate()
  // Mount the DID object to your Ceramic object
  ceramic.did = did
}
```

## Common use-cases

### Authenticate the user

!!! note ""

    This will flow will vary slightly depending on which account provider library you use. Please see the documentation specific to your provider library.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

// `seed` must be a 32-byte long Uint8Array
async function createJWS(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate the DID with the provider
  await did.authenticate()
  // This will throw an error if the DID instance is not authenticated
  const jws = await did.createJWS({ hello: 'world' })
}
```

### Enable Ceramic transactions

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

const ceramic = new CeramicClient()

// `seed` must be a 32-byte long Uint8Array
async function authenticateCeramic(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate the DID with the provider
  await did.authenticate()
  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}
```

### Resolve a DID document

```ts
import { DID } from 'dids'
import { getResolver } from 'key-did-resolver'

// See https://github.com/decentralized-identity/did-resolver
const did = new DID({ resolver: getResolver() })

// Resolve a DID document
await did.resolve('did:key:...')
```

### Store signed data on IPFS using DagJWS

The DagJWS functionality of the DID library can be used in conjunction with IPFS.

```ts
const payload = { some: 'data' }

// sign the payload as dag-jose
const { jws, linkedBlock } = await did.createDagJWS(payload)

// put the JWS into the ipfs dag
const jwsCid = await ipfs.dag.put(jws, {
  format: 'dag-jose',
  hashAlg: 'sha2-256',
})

// put the payload into the ipfs dag
const block = await ipfs.block.put(linkedBlock, { cid: jws.link })

// get the value of the payload using the payload cid
console.log((await ipfs.dag.get(jws.link)).value)
// output:
// > { some: 'data' }

// alternatively get it using the ipld path from the JWS cid
console.log((await ipfs.dag.get(jwsCid, { path: '/link' })).value)
// output:
// > { some: 'data' }

// get the jws from the dag
console.log((await ipfs.dag.get(jwsCid)).value)
// output:
// > {
// >   payload: 'AXESINDmZIeFXbbpBQWH1bXt7F2Ysg03pRcvzsvSc7vMNurc',
// >   signatures: [
// >     {
// >       protected: 'eyJraWQiOiJkaWQ6Mzp1bmRlZmluZWQ_dmVyc2lvbj0wI3NpZ25pbmciLCJhbGciOiJFUzI1NksifQ',
// >       signature: 'pNz3i10YMlv-BiVfqBbHvHQp5NH3x4TAHQ5oqSmNBUx1DH_MONa_VBZSP2o9r9epDdbRRBLQjrIeigdDWoXrBQ'
// >     }
// >   ],
// >   link: CID(bafyreigq4zsipbk5w3uqkbmh2w2633c5tcza2n5fc4x45s6soo54ynxk3q)
// > }
```

##### How it Works

As can be observed above, the createDagJWS method takes the payload, encodes it using dag-cbor, and computes its CID. It then uses this CID as the payload of the JWS that is then signed. The JWS that was just created can be put into ipfs using the dag-jose codec. Returned is also the encoded block of the payload. This can be put into ipfs using ipfs.block.put. Alternatively, ipfs.dag.put(payload) would have the same effect.

### Store encrypted data on IPFS with DagJWE

The DagJWE functionality allows us to encrypt IPLD data to one or multiple DIDs. The resulting JWE object can then be put into ipfs using the dag-jose codec. A user that is authenticated can at a later point decrypt this object.

```ts
const cleartext = { some: 'data', coolLink: new CID('bafyqacnbmrqxgzdgdeaui') }

// encrypt the cleartext object
const jwe = await did.createDagJWE(cleartext, [
  'did:3:bafy89h4f9...',
  'did:key:za234...',
])

// put the JWE into the ipfs dag
const jweCid = await ipfs.dag.put(jwe, {
  format: 'dag-jose',
  hashAlg: 'sha2-256',
})

// get the jwe from the dag and decrypt it
const dagJWE = await ipfs.dag.get(jweCid)
console.log(await did.decryptDagJWE(dagJWE))
// output:
// > { some: 'data' }
```

<!--
## A Note on Wallets

---

## A Note on Encryption and Privacy

---

## Guides

---

- [Choosing the right account type for your Ceramic project]()
- [How to store signed and encrypted data on IPFS (and Ceramic)]()

## Additional Resources

---

- [How encryption works on Ceramic]()
- [How to approach privacy and encryption on Ceramic]()
- [Meet DagJOSE: the decentralized linked-data format that powers Ceramic]()
- [EIP-2844: Ethereum Improvement Proposal for a standard DID interface]()
- [W3C: Decentralized Identifiers (DIDs) 1.0 Specification]()

## Next Steps

---

- To support transactions, you'll need to set up your DID provider for authentication.
-->
