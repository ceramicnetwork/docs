# CAIP10-Link.js

CAIP-10 Link is a StreamType that stores a cryptographically verifiable proof that links a blockchain address to a DID.

##### Things to Know

- A DID can have an unlimited number of CAIP-10 Links that publicly bind it to many different addresses on many different L1 and L2 blockchain networks.
- TileDocuments rely on [anchor commits](../../learn/glossary.md#anchor-commit) for providing immutable timestamps for the [genesis commit](../../learn/glossary.md#genesis-commit) and subsequent [signed commits](../../learn/glossary.md#signed-commit) in the stream. In the case of conflicting versions, the branch with the earliest recorded anchor commit will be respected as the canonical branch.
- TileDocuments rely on [DIDs](../../learn/glossary.md#dids) for [authentication](../../learn/glossary.md#authentication). Only the DID(s) assigned as the [controller](../../learn/glossary.md#controllers) of the stream are allowed to perform writes.
- As more updates are made to a single tile, the underlying DAG grows linearly, and so does sync times when fetching the stream over the network. When loading a tile from a node that already has it present, reqponses are very quick
- This guide describes how to create, update, and query [TileDocuments](./overview.md) using the [JS HTTP Client](../../build/javascript/installation.md#js-http-client) and the [Core Client](../../build/javascript/installation.md#js-core-client). You can also interact with TileDocuments from the [CLI](../../build/cli/installation.md); see the [Quick Start](../../build/cli/quick-start.md) guide for more information.
- Authentication: DIDs
- Ordering: Anchor records checkpointed into a blockchain (via blockheight)
- Conflict resolution: Earliest anchor wins (EAW)

## Installation

---

> **REQUIREMENTS** 
>
> You need an [installed client](../../build/javascript/installation.md), [authenticated user](../../build/javascript/authentication.md), and a third-party blockchain provider (i.e. wallet) to perform writes to Caip10Links. If you only wish to query Caip10Links then you only need an installed client.

First, install CAIP10Link.js from npm:

``` sh
npm install @ceramicnetwork/stream-caip10-link
```

Then, include CAIP10Link.js in your project:

```ts
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'
```

## Explore the API

---

### [Create a new link →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#fromaccount){:target="\_blank"}

Use the `Caip10Link.fromAccount()` method to create a new Caip10Link for a given Caip10-formatted blockchain address. The newly created Caip10Link will _not_ have any DID associated with it at first.

``` ts
await Caip10Link.fromAccount(ceramic, accountId, opts)
```

##### Parameters

`ceramic` – When creating a Caip10Link, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic-1.html){:target="\_blank"} when using the JS Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="\_blank"} when using the JS HTTP client.

`accountID` – The blockchain account being linked, in CAIP10 format.

[`createOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="\_blank"} (*Optional*)– Instructions for what actions a Ceramic node should take after receiving the transaction from your client. Note, these options are not included in the link itself.

| Parameter            | Required? | Value   | Description                                                                    | Default value            |
| -------------------- | --------- | ------- | ------------------------------------------------------------------------------ | ------------------------ |
| `anchor`             | optional  | boolean | Request an anchor after creating the link                                      | true                     |
| `publish`            | optional  | boolean | Publish the new link to the network                                            | true                     |
| `sync`               | optional  | enum    | Controls behavior related to syncing the current link state from the network   | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional  | number  | How long to wait to hear about the current state of the link from the network  | 3                        |
| `pin`                | optional  | boolean | Whether to immediately pin the stream upon creation on the connected node      | false                    |

!!! warning ""

    By default `Caip10Link.fromAccount()` will try to query the network for the current stream state. If you know that this is the first time a CAIP10Link has been created for this blockchain address and you wish to avoid unnecessarily waiting on a response from the network, you can set `syncTimeoutSeconds` to 0 in the `opts` argument.

---

### [Set DID to the link →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#setdid){:target="\_blank"}

Use the `link.setDid()` method to set or update the DID associated with a given CAIP10Link.

```ts
await link.setDid(did, authProvider, opts)
```

!!! warning "Using the `deterministic` function"

    For most use cases you will likely want to use [`TileDocument.create()`](#create-a-tiledocument). However for special circumstances, you may want to use [`TileDocument.deterministic()`](#create-a-deterministic-tiledocument) . For example you should use [`TileDocument.deterministic()`](#create-a-deterministic-tiledocument)  if you would like to enable [deterministic queries](#query-a-deterministic-tiledocument) for your TileDocument. If this is your use case, then it is also important that you provide initial metadata as deterministic document queries are based entirely on the document's initial metadata. You can proceed to add content to your document by [updating it](#update-a-tiledocument).

##### Parameters

`did` – the DID to associate with the caip10 blockchain account represented by this Caip10Link.

[`authProvider`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_blockchain_utils_linking.authprovider-1.html) – an instance of the AuthProvider API object that can create link proofs for the blockchain network that the CAIP10 account lives on.

[`updateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="\_blank"} – Instructions for what actions a Ceramic node should take after receiving the transaction from your client. Note, these options are not included in the link itself.

| Parameter | Required? | Value   | Description                               | Default value |
| --------- | --------- | ------- | ----------------------------------------- | ------------- |
| `anchor`  | optional  | boolean | Request an anchor after updating the link | true          |
| `publish` | optional  | boolean | Publish the update to the network         | true          |

---

### [Load a link →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#fromaccount){:target="\_blank"}

Use the `Caip10Link.fromAccount()` method to look up a link for a given CAIP10 blockchain address.

``` ts
await Caip10Link.fromAccount(ceramic, accountId, opts)
```

!!! warning ""

    The examples for [creating a new Caip10Link](#create-new-caip10link) and loading an existing Caip10Link look the same. `Caip10Link.fromAccount` will create a new link if one doesn't exist, in which case the returned link will have no linked DID associated with it. If the link already exists, however, then `Caip10Link.fromAccount` will return the current state of the link, which may include a linked DID if one has been set previously.

##### Parameters

The parameters for loading a link are the same as those for creating a new link, as described above.


## Examples

---

### Create a Link
Here we create a new empty Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`).

``` ts
const accountLink = await Caip10Link.fromAccount(
  ceramic,
  '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8@eip155:1',
)
```

Here we can see the full flow of getting a user's Ethereum address, creating a link, and adding the users' DID account. In this example we create a Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`) and then associate it with the DID `did:3:k2t6...ydki`.

``` ts
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'
import { EthereumAuthProvider } from '@ceramicnetwork/blockchain-utils-linking'

// First, get an ethereum provider for communicating with the ethereum blockchain.
// This example assumes you have an ethereum provider available in `window.ethereum`, provided
// by the web browser or a browser extension.
const ethProvider = window.ethereum
// Next use the ethProvider to make an EthereumAuthProvider which can issue LinkProofs linking
// addresses on Ethereum to DIDs.
const ethAuthProvider = new EthereumAuthProvider(
  ethProvider,
  '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8',
)
const accountId = await ethAuthProvider.accountId()

const accountLink = await Caip10Link.fromAccount(ceramic, accountId)
await accountLink.setDid(
  'did:3:k2t6wyfsu4pg0t2n4j8ms3s33xsgqjhtto04mvq8w5a2v5xo48idyz38l7ydki',
  ethAuthProvider,
)
```

### Load a link

In this example we load a Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`).

``` ts
const accountLink = await Caip10Link.fromAccount(
  ceramic,
  '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8@eip155:1',
)
const linkedDid = accountLink.did
```

## Additional Resources

---

- [CIP-10: CAIP10 Link Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md)
- [Complete CAIP10Link.js API Reference]()

## Next Steps

---

- [Next step 1]()