# JS Core Client
The JS Core Client allows you to run the full Ceramic protocol (API and node) directly in any JavaScript environment, such as in your tests, in fully client-side browser applications, or in node.js. Carefully read the [considerations](#considerations) below to decide if the JS Core Client is right for your project. Most applications instead use the [JS HTTP Client](./http.md).

[:octicons-download-16: Installation](#installation) | [:octicons-file-code-16: Full API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"}

## **Considerations**

**Maximal security and decentralization**: The Ceramic Core client does not have trusted relationships with any external nodes. With Ceramic Core, streams that are [written](../../build/writes.md), [queried](../../build/queries.md), or [pinned](../../build/pinning.md) are verified in the local environment which is great if you need maximal security and decentralization in your application. 

**Transitory data availability**: Streams created with Ceramic Core will only be available on the network as long as this node remains online. For example for setups that use the Core Client directly in-browser, when your user closes the tab any stream created by that user will become unavailable  on the network until the user opens the application again. For more resilient data availability you can always replicate and pin streams on secondary long-running nodes, or instead use the [JS HTTP Client](./http.md) which relies on a remote node more likely to always be online.

**Setup complexity**: You will need to configure an [IPFS](../../learn/glossary.md#ipfs) node which supports the [dag-jose](../../learn/glossary.md#dagjose) data format and ensure connectivity to the rest of the Ceramic network. See [installation](#installation) below for instructions on how to do this.

**Swap for JS HTTP Client at any time**: The JS Core Client and the [JS HTTP Client](./http.md) implement the same [CeramicApi](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="_blank"} TypeScript interface, so swapping between clients is seamless and doesn't require changing your application logic; it only requires changing your setup.

## **Installation**
Installing the JS Core Client requires a console, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

    !!! warning ""
    While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
    
    ```bash
    npm install -g node-pre-gyp
    ```
    
    *This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).*

### 1. Install the Core client
Open your console and install the JS Core Client using npm.

``` bash
npm install @ceramicnetwork/core
```

### 2. Import the Core client

``` javascript
import Ceramic from '@ceramicnetwork/core'
```

### 3. Import IPFS with dag-jose
Ceramic utilizes the [dag-jose](../../learn/glossary.md#dagjose) IPLD codec to format and store data in IPFS.

``` javascript
import IPFS from 'ipfs'
import dagJose from 'dag-jose'
import { sha256 } from 'multiformats/hashes/sha2'
import legacy from 'multiformats/legacy'

const hasher = {}
hasher[sha256.code] = sha256
const dagJoseFormat = legacy(dagJose, {hashes: hasher})

```

### 4. Create an IPFS instance
Create an instance of `js-ipfs` with `dag-jose` enabled.

``` javascript
const ipfs = await Ipfs.create({ ipld: { formats: [dagJoseFormat] } })
```

### 5. Create a Ceramic instance
Create an instance of Ceramic by passing ipfs and an optional configuration object.

``` javascript
const ceramic = await Ceramic.create(ipfs, config)
```

### 6. Import DID resolvers
Import resolvers for all DID methods that this Core Client will support. This should be inclusive of the DID Method that you will use for [authentication](../../build/authentication.md), but should also include all other DID Methods for which your node could possibly need to verify signatures. Therefore, it is recommended that all Core Clients be able to resolve at least the `did:3` and `did:key` DID methods.


``` javascript
import KeyDidResolver from 'key-did-resolver'
import ThreeIdResolver from '@ceramicnetwork/3id-did-resolver'
```

### 7. Create a resolver instance
This should include all DID resolvers from the previous step.

``` javascript
const resolver = { ...KeyDidResolver.getResolver(),
                   ...ThreeIdResolver.getResolver(ceramic) }
```

### 8. Create a DID instance
Create a DID instance which wraps the resolver. Optionally, it also includes a DID Provider if you intend to [authenticate](../../build/authentication.md) DIDs to allow [writes](../../build/writes.md) to the network during runtime.

``` javascript
import { DID } from 'dids'
const did = new DID({ resolver })
```

### 9. Set DID instance on Core client

``` javascript
ceramic.setDID(did)
```

## **Next steps**
After setting the DID instance on the Core client, your application will now be able to perform [queries](../../build/queries.md). If you need to perform writes, proceed to setting up [authentication](../../build/authentication.md).


</br>
</br>
</br>
