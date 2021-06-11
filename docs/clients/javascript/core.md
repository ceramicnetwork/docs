# JS Core Client
The JS Core Client allows you to run the full Ceramic protocol (API and node) directly in any JavaScript environment, including directly in-browser. You might use the Core client if you want your project to be maximally decentralized. However, there are tradeoffs such as performance and data availability when using it in browser since this node can come on and offline as the user opens and closes browser windows.

[:octicons-download-16: Installation](#installation) | [:octicons-file-code-16: Full API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"}

## **Benefits**

**Maximal decentralization**:

**Strong security**:

**No external node needed**:

## **Installation**
Installing the JS Core Client requires a terminal, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

    !!! warning ""
    While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
    
    ```bash
    npm install -g node-pre-gyp
    ```
    
    *This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).*

### 1. Install the Core client
Open your terminal and install the JS Core Client using npm.

``` bash
npm install @ceramicnetwork/core
```

### 2. Import the Core client

``` javascript
import Ceramic from '@ceramicnetwork/core'
```

### 3. Import IPFS with dag-jose
Ceramic utilizes the [dag-jose](https://github.com/ipld/specs/blob/master/block-layer/codecs/dag-jose.md){:target="_blank"} IPLD codec to store data in IPFS.

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

### 6. Provide a DID instance to the Core client
Ceramic instances need access to a DID instance to use to resolve DIDs and to create and validate cryptographic signatures on Ceramic commits. See the [Configure your DID](configure-did.md) page for more information on how to do this.


## **Next steps**
### Authenticate to perform writes
If you need to perform [writes](), then you will next need to [Configure a DID]() then [Authenticate](authentication.md). If you only need to query streams, you can skip ahead to [queries]().

</br></br></br>
