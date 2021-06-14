# JS HTTP Client
The JS HTTP Client is a lightweight way of interacting with the Ceramic netwotk. It allows your JavaScript application to connect to a remote Ceramic node over HTTP to [read]() and [write]() streams. The main decision to make when using the JS HTTP Client is which remote Ceramic node to use. The JS HTTP client is recommended when building most applications.

[:octicons-download-16: Installation](#installation) | [:octicons-file-code-16: Full API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="_blank"}

## **Considerations**

**Improved performance**: When using the JS HTTP Client, stream processing and validation happens on a remote Ceramic node running on a server, which usually results in improved performance compared to running the full protocol in-browser with the [JS Core Client](./core.md).

**Predictable data availability**: Streams created using the JS HTTP Client can be pinned and made available on a remote Ceramic node which has more uptime and predictable data availability guarantees than, say, running the [JS Core Client](./core.md) directly in-browser where users can open and close tabs causing their streams to come on and offline at unpredictable intervals.

**Some trust in a remote node**: Stream processing and state validation happens on a remote node which the JS HTTP Client trusts. However, it is important to note that user's keys always live client-side and all updates are signed on the JS HTTP Client and then sent to the HTTP endpoint for processing.

**Swap for the JS Core Client at anytime**: The JS HTTP Client and the [JS Core Client](./core.md) implement the same [CeramicApi](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="_blank"} TypeScript interface, so swapping between clients is seamless and doesn't require changing your application logic; it only requires changing your setup.

## **Installation**

Installing the JS HTTP Client requires a console, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

    !!! warning ""
    While npm v7 is not officially supported, you may still be able to get it to work. However you will need to install the `node-pre-gyp` package globally. This is required until `node-webrtc`, which [IPFS](../../learn/glossary.md#ipfs) depends on, [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694){:target="_blank"}.
    
    ```bash
    npm install -g node-pre-gyp
    ```

### 1. Install the HTTP client
Open your console and install the JS HTTP Client using npm.

``` bash
npm install @ceramicnetwork/http-client
```

### 2. Import the HTTP client

``` javascript
import CeramicClient from '@ceramicnetwork/http-client'
```

### 3. Configure your node URL

``` javascript
const API_URL = "https://yourceramicnode.com"
```

Available options for your node setup:

- [Free community nodes](../../tools/hosted-nodes/community-nodes.md): Discover free HTTP endpoints
- [Commercial node providers](../../tools/hosted-nodes/node-providers.md): Discover paid node providers
- [Host your own node](../../run/nodes.md): Learn how to setup and host your own node
- *localhost*: You may want to use the JS HTTP Client with a Ceramic node running on your local machine for development and testing. To achieve this, first start a local daemon by [installing the CLI](./cli.md). Once the CLI is installed, you can pass `https://localhost:7007` to the HTTP client. This setup will allow you to read streams from other nodes connected on the Ceramic network, but writes to your local node will only be available on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json){:target="_blank"}. For now, these streams will not be available to other nodes on the network due to a limitation in `js-ipfs` which will be fixed in the future.

### 4. Create a Ceramic instance

``` javascript
const ceramic = new CeramicClient(API_URL)
```

### 5. Import DID resolvers
Import resolvers for all DID methods that will perform [writes](../../build/writes.md) using this HTTP Client. This should include all DID Methods that your app will use for [authentication](../../build/authentication.md). If your HTTP Client will only perform queries, then jump straight to the [queries](../../build/queries.md) page.

``` javascript
import KeyDidResolver from 'key-did-resolver'
import ThreeIdResolver from '@ceramicnetwork/3id-did-resolver'
```

### 6. Create a resolver instance
This should include all DID resolvers included in the previous step.

``` javascript
const resolver = { ...KeyDidResolver.getResolver(),
                   ...ThreeIdResolver.getResolver(ceramic) }
```

### 7. Create a DID instance
The DID instance wraps the resolver and also should include the DID Provider for the DID Method your users will use to [authenticate](../../build/authentication.md) to perform [writes](../../build/writes.md).

``` javascript
import { DID } from 'dids'
const did = new DID({ resolver })
```

### 8. Set DID instance on HTTP client

``` javascript
ceramic.did = did
```

## **Next steps**
After setting the DID instance on the HTTP client, your application will be able to support writes. Proceed to [setting up authentication](../../build/authentication.md) so users can authenticate to perform these writes. If your app only needs to perform queries, then jump ahead to [queries](../../build/queries.md).


</br>
</br>
</br>
