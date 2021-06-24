# Installing the JS HTTP Client
This guide descibes how to install the [JS HTTP Client](./installation.md#js-http-client) so your JavaScript application can perform [writes](./writes.md), [queries](./queries.md), or [pins](./pinning.md) on a remote Ceramic node during runtime.

## **Requirements**

Installing the JS HTTP Client requires a console, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

!!! warning ""

    While npm v7 is not officially supported, you may still be able to get it to work. However you will need to install the `node-pre-gyp` package globally. This is required until `node-webrtc`, which [IPFS](../../learn/glossary.md#ipfs) depends on, [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694){:target="_blank"}.
    
    ```bash
    npm install -g node-pre-gyp
    ```

## **1. Install the HTTP client**
Open your console and install the JS HTTP Client using npm.

``` bash
npm install @ceramicnetwork/http-client
```

## **2. Import the HTTP client**

``` javascript
import CeramicClient from '@ceramicnetwork/http-client'
```

## **3. Configure your node URL**

``` javascript
const API_URL = "https://yourceramicnode.com"
```

Available options for your node setup:

- [Free community nodes](../../tools/hosted-nodes/community-nodes.md): Discover free HTTP endpoints
- [Commercial node providers](../../tools/hosted-nodes/node-providers.md): Discover paid node providers
- [Host your own node](../../run/nodes/nodes.md): Learn how to setup and host your own node
- *localhost*: You may want to use the JS HTTP Client with a Ceramic node running on your local machine for development and testing. To achieve this, first start a local daemon by [installing the CLI](../cli/installation.md). Once the CLI is installed, you can pass `https://localhost:7007` to the HTTP client. This setup will allow you to read streams from other nodes connected on the Ceramic network, but writes to your local node will only be available on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json){:target="_blank"}. For now, these streams will not be available to other nodes on the network due to a limitation in `js-ipfs` which will be fixed in the future.

## **4. Create a Ceramic instance**

``` javascript
const ceramic = new CeramicClient(API_URL)
```

## **5. Import DID resolvers**
Import the DID resolvers for all DID methods that will need to [authenticate](./authentication.md) to perform [writes](./writes.md) using this HTTP Client. If your HTTP Client will only perform queries, then jump ahead to the [queries](./queries.md) page.

``` javascript
import KeyDidResolver from 'key-did-resolver'
import ThreeIdResolver from '@ceramicnetwork/3id-did-resolver'
```

## **6. Create a DID instance**
Create a DID instance which wraps an instance of a DID resolver that includes all individual DID resolvers from the previous step. It should also include a DID Provider for the DID Method you are using for [authentication](./authentication.md).

``` javascript
import { DID } from 'dids'
const resolver = { ...KeyDidResolver.getResolver(),
                   ...ThreeIdResolver.getResolver(ceramic) }
const did = new DID({ resolver })
```

## **7. Set DID instance on HTTP client**

``` javascript
ceramic.did = did
```

## **Next steps**
If your application needs to perform writes, proceed to [setting up authentication](./authentication.md). If your app only needs to perform queries, then jump ahead to [queries](./queries.md).
