# JavaScript HTTP Client
The JS HTTP clent allows your JavaScript application to interact with a remote Ceramic node. The JS HTTP client is a lightweight way of interacting with the Ceramic network. It works by connecting to an HTTP endpoint provided by a remote Ceramic node to [read]() and [write]() streams. The main consideration when using the JS HTTP Client is deciding which remote Ceramic node to use.

The JS HTTP client is recommended when building most applications.

[:octicons-download-16: Installation](#installation) | [:octicons-file-code-16: Full API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="_blank"}

## **Benefits**

**Improved performance**: This means that state validation happens in the remote node which the client trusts. 

**More predictable data availability**:

**No security tradeoffs**: Important to note however is the user's keys always live client side and all updates are authored on the client and then sent to the remote http endpoint to write the update to the network.

**Swap for the JS Core Client anytime**: The HTTP client implements the standard *CeramicApi*, so it can easily be interchanged for the [JS Core Client]() if you decide to switch at a later time.

## **Installation**

This installation guide will use a terminal, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
```bash
npm install -g node-pre-gyp
```
This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).

### 1. Install the client
Open your terminal and install the JS HTTP Client using npm.

``` bash
npm install @ceramicnetwork/http-client
```

### 2. Import the client

``` javascript
import CeramicClient from '@ceramicnetwork/http-client'
```

### 3. Connect to a remote node

``` javascript
const API_URL = "http://yourceramicnode.com"
```

Available options for your node setup:

- [Free community nodes](../tools/hosted-nodes/community-nodes.md): Discover free HTTP endpoints
- [Commercial node providers](../tools/hosted-nodes/node-providers.md): Discover paid node providers
- [Host your own node](../run/nodes.md): Learn how to setup and host your own node
- *localhost*: To use the JS HTTP Client with a Ceramic node running on your local machine, you will need to first start a local daemon by [installing the CLI](). Once the CLI is installed, you can pass `https://localhost:7007` to the HTTP client. This setup will allow you to read streams from other nodes connected on the Ceramic network, but writes to your local node will only be available on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json), but will not be available to other nodes on the network (this is a limitation with js-ipfs and will be fixed in the future).

### 4. Create an instance

``` javascript
const ceramic = new CeramicClient(API_URL)
```

## **Next steps**

### Authenticate your client to perform writes
The JS HTTP Client needs access to a DID instance to create and validate cryptographic signatures on stream [commits](). If you need to perform [writes](), next you'll need to [Configure a DID](configure-did.md) then [Authenticate](authentication.md) it. *If you only need to perform queries, then you can skip these steps and jump directly to [querying streams]().* 

</br></br></br>
