# Caip10Link API

This guide demonstrates how to create, update, and query Caip10Links using the [JS HTTP Client](../../clients/javascript/http.md) and [JS Core Client](../../clients/javascript/core.md).

## **Requirements**

You need an [installed client](../../build/installation.md) to read or write Caip10Links.

## **Write API**

### **Create new Caip10Link**

Use the [`Caip10Link.fromAccount()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#fromaccount){:target="_blank"} method to create a new Caip10Link for a given Caip10-formatted blockchain address. The newly created Caip10Link will *not* have any DID associated with it at first.

```javascript
const link = await Caip10Link.fromAccount(ceramic, accountId, opts)
```

In this example we create a new empty Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`).

```javascript
const accountLink = await Caip10Link.fromAccount(
    ceramic,
    '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8@eip155:1',
)
```

#### Parameters

##### `ceramic`

When creating a Caip10Link, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"} when using the JS Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="_blank"} when using the JS HTTP client.

##### `accountId`

The account ID in Caip10 format of the blockchain account being linked.

##### `opts`  |  optional

The final argument is an instance of [`CreateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="_blank"}, which are options that control network behaviors performed as part of the operation.  They are not included in the link itself.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after creating the link | true |
| `publish`     | optional    | boolean          | Publish the new link to the network | true |
| `sync`        | optional    | enum             | Controls behavior related to syncing the current link state from the network | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional    | number            | How long to wait to hear about the current state of the link from the network | 3 |

!!! warning ""

    By default `Caip10Link.fromAccount()` will try to query the network for the current stream state. If you know that this is the first time a Caip10Link has been created for this blockchain address, and you wish to avoid waiting on a response from the network unnecessarily, you can set `syncTimeoutSeconds` to 0 in the `opts` argument.


### **Set DID to Caip10Link**

Use the [`link.setDid()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#setdid){:target="_blank"} method to set or update the DID associated with a given Caip10Link.

```javascript
const link = await Caip10Link.fromAccount(ceramic, accountId, opts)
await link.setDid(did, authProvider, opts)
```

In this example we create a Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`) and then associate it with the DID `did:3:k2t6...ydki`.

```javascript
import { Caip10Link } from '@ceramicnetwork/stream-caip10-link'
import { EthereumAuthProvider } from '@ceramicnetwork/blockchain-utils-linking'

// First, get an ethereum provider for communicating with the ethereum blockchain.
// This example assumes you have an ethereum provider available in `window.ethereum`, provided
// by the web browser or a browser extension.
const ethProvider = window.ethereum
// Next use the ethProvider to make an EthereumAuthProvider which can issue LinkProofs linking
// addresses on Ethereum to DIDs.
const ethAuthProvider = new EthereumAuthProvider(
    ethProvider, '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8')
const accountId = await ethAuthProvider.accountId()

const accountLink = await Caip10Link.fromAccount(ceramic, accountId)
await accountLink.setDid(
    'did:3:k2t6wyfsu4pg0t2n4j8ms3s33xsgqjhtto04mvq8w5a2v5xo48idyz38l7ydki',
    ethAuthProvider)
```

#### Parameters

##### `did`

The DID to associate with the caip10 blockchain account represented by this Caip10Link.

##### `authProvider`

An instance of the [`AuthProvider`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_blockchain_utils_linking.authprovider-1.html) interface that can create link proofs for the blockchain network that the Caip10 account lives on.


##### `opts`  |  optional
The final argument is an instance of [`UpdateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="_blank"}, which are options that control network behaviors performed as part of the operation.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after updating the link | true |
| `publish`     | optional    | boolean          | Publish the update to the network | true |


## **Read API**

### **Load a Caip10Link**

Use the [`Caip10Link.fromAccount()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_caip10_link.caip10link-1.html#fromaccount){:target="_blank"} method to look up a Caip10Link for a given Caip10 blockchain address.

```javascript
const link = await Caip10Link.fromAccount(ceramic, accountId, opts)
```

In this example we load a Caip10Link for the account `0x054...7cb8` on the Ethereum mainnet blockchain (`eip155:1`).

```javascript
const accountLink = await Caip10Link.fromAccount(
    ceramic,
    '0x0544dcf4fce959c6c4f3b7530190cb5e1bd67cb8@eip155:1',
)
const linkedDid = accountLink.did
```

The examples for [creating a new Caip10Link](#create-new-caip10link) and loading an existing Caip10Link look the same. `Caip10Link.fromAccount` will create a new link if one doesn't exist, in which case the returned link will have no linked DID associated with it. If the link already exists, however, then `Caip10Link.fromAccount` will return the current state of the link, which may include a linked DID if one has been set previously.

#### Parameters

The parameters for loading a Caip10Link are the same as those for [creating a new Caip10Link](#create-new-caip10link).

</br>
</br>
</br>
