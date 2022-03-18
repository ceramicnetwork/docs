# Installing the JS Core Client

This guide describes how to install the [JS Core Client](./installation.md#js-core-client) in your JavaScript project.

## **Requirements**

Installing the JS Core Client requires a console, [Node.js](https://nodejs.org/en/){:target="\_blank"} v16, and [npm](https://www.npmjs.com/get-npm){:target="\_blank"} v6. Make sure to have these installed on your machine.

!!! warning ""

    While npm v7 is not officially supported, you may still be able to get it to work. Try installing the `node-pre-gyp` package globally. This is required until `node-webrtc`, which IPFS depends on, [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).

    ```bash
    npm install -g node-pre-gyp
    ```

## **1. Install the Core client**

Open your console and install the JS Core Client using npm.

```bash
npm install @ceramicnetwork/core
```

## **2. Import the Core client**

In the javascript code that your application will use to connect to Ceramic, import the Ceramic Core client.

```javascript
import Ceramic from '@ceramicnetwork/core'
```

## **3. Import IPFS with dag-jose**

Ceramic utilizes the [dag-jose](../../learn/glossary.md#dagjose) IPLD codec to format and store data in IPFS.

```javascript
import Ipfs from 'ipfs-core'
import dagJose from 'dag-jose'
import { convert } from 'blockcodec-to-ipld-format'

const dagJoseFormat = convert(dagJose)
```

## **4. Create an IPFS instance**

Create an instance of `js-ipfs` with `dag-jose` enabled.

```javascript
const ipfs = await Ipfs.create({ ipld: { formats: [dagJoseFormat] } })
```

## **5. Create a Ceramic instance**

Create an instance of Ceramic by passing ipfs and an optional configuration object.

```javascript
const ceramic = await Ceramic.create(ipfs, config)
```

## **6. Import DID resolvers**

Import resolvers for all DID methods that this Core Client will support. This should be inclusive of the DID Method that you will use for [authentication](./authentication.md), but should also include all other DID Methods for which your node could possibly need to verify signatures. Therefore, it is recommended that all Core Clients be able to resolve at least the `did:3` and `did:key` DID methods.

```javascript
import KeyDidResolver from 'key-did-resolver'
import ThreeIdResolver from '@ceramicnetwork/3id-did-resolver'
```

## **7. Create a DID instance**

Create a DID instance which wraps an instance of a DID resolver that includes all individual DID resolvers from the previous step. Optionally, it also includes a DID Provider if you intend to [authenticate](./authentication.md) DIDs to allow [writes](./writes.md) to the network during runtime.

```javascript
import { DID } from 'dids'
const resolver = {
  ...KeyDidResolver.getResolver(),
  ...ThreeIdResolver.getResolver(ceramic),
}
const did = new DID({ resolver })
```

## **8. Set DID instance on Core client**

```javascript
ceramic.did = did
```

## **Example**
Once you have completed installing and configuring the Core Client, your project's setup should look something like this.

``` javascript
import Ceramic from '@ceramicnetwork/core'
import IPFS from 'ipfs-core'
import dagJose from 'dag-jose'
import { convert } from 'blockcodec-to-ipld-format'
import KeyDidResolver from 'key-did-resolver'
import ThreeIdResolver from '@ceramicnetwork/3id-did-resolver'
import { DID } from 'dids'
const dagJoseFormat = convert(dagJose)
const ipfs = await Ipfs.create({ ipld: { formats: [dagJoseFormat] } })
const resolver = { ...KeyDidResolver.getResolver(),
                   ...ThreeIdResolver.getResolver(ceramic) }
const did = new DID({ resolver })
ceramic.did = did
```

## **Next steps**

After setting the DID instance on the Core client, your application will now be able to perform [queries](./queries.md). If you need to perform writes, proceed to setting up [authentication](./authentication.md).
