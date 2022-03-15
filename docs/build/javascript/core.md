# Installing the JS Core Client

This guide describes how to install the [JS Core Client](./installation.md#js-core-client) in your JavaScript project.

## **Requirements**

Installing the JS Core Client requires a console, [Node.js](https://nodejs.org/en/){:target="\_blank"} v16, and [npm](https://www.npmjs.com/get-npm){:target="\_blank"} v+. Make sure to have these installed on your machine.

!!! warning ""

    The Core Client is only recommended for specific use-cases where running an in-process Ceramic node is desirable. The [HTTP Client](./http.md) is recommended for most use-cases.

## **1. Install the Core client**

Open your console and install the JS Core Client using npm.

```bash
npm install @ceramicnetwork/core
```

## **2. Import the Core client**

```javascript
import { Ceramic } from '@ceramicnetwork/core'
```

## **3. Create an IPFS instance**

Create an instance of `js-ipfs`.

```javascript
import IPFS from 'ipfs-core'

const ipfs = await IPFS.create()
```

## **4. Create a Ceramic instance**

Create an instance of Ceramic by passing ipfs and an optional configuration object.

```javascript
const ceramic = await Ceramic.create(ipfs, config)
```

## **5. Import DID resolvers**

Import resolvers for all DID methods that this Core Client will support. This should be inclusive of the DID Method that you will use for [authentication](./authentication.md), but should also include all other DID Methods for which your node could possibly need to verify signatures. Therefore, it is recommended that all Core Clients be able to resolve at least the `did:3` and `did:key` DID methods.

```javascript
import { getResolver as getKeyResolver } from 'key-did-resolver'
import { getResolver as get3IDResolver } from '@ceramicnetwork/3id-did-resolver'
```

## **6. Create a DID instance**

Create a DID instance which wraps an instance of a DID resolver that includes all individual DID resolvers from the previous step. Optionally, it also includes a DID Provider if you intend to [authenticate](./authentication.md) DIDs to allow [writes](./writes.md) to the network during runtime.

```javascript
import { DID } from 'dids'

const resolver = {
  ...getKeyResolver(),
  ...get3IDResolver(ceramic),
}
const did = new DID({ resolver })
```

## **7. Set DID instance on Core client**

```javascript
ceramic.did = did
```

## **Example**

Once you have completed installing and configuring the Core Client, your project's setup should look something like this.

```javascript
import { Ceramic } from '@ceramicnetwork/core'
import IPFS from 'ipfs-core'
import { getResolver as getKeyResolver } from 'key-did-resolver'
import { getResolver as get3IDResolver } from '@ceramicnetwork/3id-did-resolver'
import { DID } from 'dids'

const ipfs = await IPFS.create()
const ceramic = await Ceramic.create(ipfs)
const resolver = {
  ...getKeyResolver(),
  ...get3IDResolver(ceramic),
}
const did = new DID({ resolver })
ceramic.did = did
```

## **Next steps**

After setting the DID instance on the Core client, your application will now be able to perform [queries](./queries.md). If you need to perform writes, proceed to setting up [authentication](./authentication.md).
