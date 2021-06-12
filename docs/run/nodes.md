# Hosting a node
This guide describes how to spin up and run a hosted Ceramic node in JavaScript that can be used as a remote node by the [JS HTTP Client](../clients/javascript/http.md) or the [CLI](../clients/javascript/cli.md). It can also be used for redundant stream pinning and replication from any other node.

## **Installation**
This guide requires the use of a console, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

While npm v7 is not officially supported, you may still be able to get it to work. You will need to install the `node-pre-gyp` package globally. This is required until `node-webrtc` which [IPFS](../learn/glossary.md#ipfs) depends on [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694){:target="_blank"}.

```bash
npm install -g node-pre-gyp
```

### 1. Install the CLI
Open your console and install the CLI.

``` bash
$ npm install -g @ceramicnetwork/cli
```

### 2. Start the Ceramic node
This starts a Ceramic node with a standard configuration on the [Clay Testnet](../learn/networks.md#clay-testnet) at `https://localhost:7007`. Behind the scenes a properly configured [IPFS](../learn/glossary.md#ipfs) instance is started, then IPFS is used to create an instance of the Ceramic [JS Core Client](../clients/javascript) connected to the [Clay Testnet](../learn/networks.md#clay-testnet). Once Ceramic Core is running, an HTTP server is started that serves the Ceramic [HTTP API](../reference/http-api.md).

```bash
$ ceramic daemon
```

### 3. Use an external IPFS node (optional)
By default the JS CLI starts a node with a bundled IPFS instance. If you would like to instead run a separate service for IPFS, use the `@ceramicnetwork/ipfs-daemon` npm package. 

### 4. Configure a network (optional)
By default the JS CLI starts a node on the Clay Testnet. If you would like to use a different network, you can specify this using the `--network` option. Currently, it is only possible to host nodes on the Clay Testnet. We will add the ability to run nodes on [Mainnet](../learn/networks.md#mainnet) in the near future.

### 5. Expose endpoints
If you want your node and the streams created on it to be discoverable to the rest of the network, make sure the appropriate ports used by IPFS and the Ceramic HTTP API (by default this is port 7007) are publicly accessible. For example, if you are behind a router you may need to set up port forwarding. 

### 6. Enable peer discovery
To enable peer discovery, add your node to the appropriate Ceramic [`peerlist`](https://github.com/ceramicnetwork/peerlist) file by submitting a pull request. This is used as a temporary measure to streamline peer discovery until `js-ipfs` supports DHT lookups.

### 7. Gateway mode
To setup your node with read-only capabilities, enable gateway mode. Users of this node will only be able to read data from the network, but writes will be disabled.

## **Next steps**
Congratulations! You have now set up a hosted Ceramic node that is ready to receive HTTP requests from the local environment, the [JS HTTP Client](../clients/javascript/http.md), the [CLI](../clients/javascript/cli.md), or to simply serve as another node for redundant stream pinning and replication.

</br></br></br>
