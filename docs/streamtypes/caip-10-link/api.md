# Caip10Link API

This guide demonstrates how to create, update, and query Caip10Links on the Ceramic network using the [HTTP](../../../reference/javascript/clients) and [core](../../../reference/javascript/clients) clients.

## Prerequisites
You need an [installed client](../../build/installation.md) and an [authenticated user](../../build/authentication.md) to perform writes to Caip10Links on the network during runtime. If you only wish to query existing Caip10Links then you still need an installed client but it doesn't need to be authenticated.

## Create or query a link
Use the [`Caip10Link.fromAccount()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#fromAccount){:target="_blank"} method to create a new Caip10Link for a given Caip10 blockchain address, or to look up an existing link for that address.

```javascript
const link = await Caip10Link.fromAccount(ceramic, accountId, opts)
```

#### Creating a new link

In this example we create a new empty Caip10Link for the account *0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8* on the Ethereum mainnet blockchain. The newly created Caip10Link will not have any DID associated with it at first.

```javascript
const accountLink = await Caip10Link.fromAccount(
    ceramic,
    '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8@eip155:1',
    { syncTimeoutSeconds: 0 },
)
```

!!! info ""
    Note that by default `Caip10Link.fromAccount()` will try to query the network for the current Stream state. If you know that this is the first time a Caip10Link has been created for this blockchain address, and you wish to avoid waiting on a response from the network unnecessarily, you can set `syncTimeoutSeconds` to 0 in the `opts` argument, as we do in the above example.

#### Loading an existing link

In this example we load a Caip10Link for the account *0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8* on the Ethereum mainnet blockchain.

```javascript
const accountLink = await Caip10Link.fromAccount(
    ceramic,
    '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8@eip155:1',
    { anchor: false, publish: false },
)
const linkedDid = accountLink.did
```

!!! info ""
    Note that by default `Caip10Link.fromAccount()` will create a new link Stream for the given blockchain address, if no such link exists already. In the example above we set the `CreateOpts` to `{ anchor: false, publish: false }` to prevent that from happening and force it to only return results if a link already exists.

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#fromAccount){:target="_blank"}


### Parameters

#### ceramic


When creating or querying a Caip10Link, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="_blank"} when using the HTTP client.

#### accountId

The blockchain account ID - in Caip10 format - of the blockchain account being linked.

#### opts (optional)
The final argument to `Caip10Link.fromAccount` is an instance of [`CreateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="_blank"}, which are options that control network behaviors performed as part of the operation.  They are not included in the link itself.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after creating the link | true |
| `publish`     | optional    | boolean          | Publish the new link to the network | true |
| `sync`        | optional    | enum             | Controls behavior related to syncing the current link state from the network | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional    | number            | How long to wait to hear about the current state of the link from the network | 3 |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="_blank"}


## Associate a DID with a Caip10Link
Use the [`link.setDid()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#setdid){:target="_blank"} method to update the DID associated with a given Caip10Link

```javascript
const link = await Caip10Link.fromAccount(ceramic, accountId, opts)
await link.setDid(did, authProvider, opts)
```

**Example**

In this example we create a Caip10Link for the ethereum mainnet account *0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8* and then associate it with the DID *did:3:k2t6wyfsu4pg0t2n4j8ms3s33xsgqjhtto04mvq8w5a2v5xo48idyz38l7ydki*

```javascript
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'
import { EthereumAuthProvider } from '@ceramicnetwork/blockchain-utils-linking'

const ethProvider = // [...] An ethereum provider for communicating with the ethereum blockchain
const ethAuthProvider = new EthereumAuthProvider(
    ethProvider, '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8')
const accountId = await ethAuthProvider.accountId()

const accountLink = await Caip10Link.fromAccount(ceramic, accountId)
await accountLink.setDid(
    'did:3:k2t6wyfsu4pg0t2n4j8ms3s33xsgqjhtto04mvq8w5a2v5xo48idyz38l7ydki',
    ethAuthProvider)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#setdid){:target="_blank"}

### Parameters

#### did

The DID to associate with the caip10 blockchain account represented by this Caip10Link.

#### authProvider

An instance of the [`AuthProvider`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_blockchain_utils_linking.authprovider-1.html) interface that can create link proofs for the blockchain network that the account this Caip10Link represents lives on.


#### opts (optional)
The final argument to `Caip10Link.setDid` is an instance of [`UpdateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="_blank"}, which are options that control network behaviors performed as part of the operation.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after updating the link | true |
| `publish`     | optional    | boolean          | Publish the update to the network | true |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="_blank"}


</br>
</br>
</br>
