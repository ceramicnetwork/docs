# Hosting a node
This guide describes how to spin up and run a hosted Ceramic JS node that can be used as a remote node by the [JS HTTP Client](../clients/javascript/http.md) or the [CLI](../clients/javascript/cli.md). It can also be used for redundant stream pinning and replication from any other node. Currently, it is only possible to run nodes on the [Clay Testnet](../learn/networks.md#clay-testnet). We will add the ability to run nodes on [Mainnet](../learn/networks.md#mainnet) in the near future.

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

### 2. Start the node

```bash
$ ceramic daemon
```

This starts a Ceramic node with a standard cofiguration. By default the daemon runs a bundled [IPFS](../learn/glossary.md#ipfs) instance. Behind the scenes a properly configured IPFS instance is started, then IPFS is used to create an instance of the Ceramic [JS Core Client](../clients/javascript). Once Ceramic Core is running, an HTTP server is started that serves the Ceramic HTTP API. The node is now ready to accept requests from the local environment or from a remote [JS HTTP Client](../clients/javascript/http.md) or [CLI](../clients/javascript/cli.md).

#### Using an external IPFS node

It's also possible to run a separate service for IPFS using the `@ceramicnetwork/ipfs-daemon` npm package. 

### 3. Expose ports

If you want your node and the streams created on it to be discoverable to the rest of the network, make sure the appropriate ports used by IPFS and the Ceramic HTTP API (by default this is port 7007) are publicly accessible. For example, if you are behind a router you may need to set up port forwarding. 

### 4. Enable peer discovery

To enable peer discovery, add your node to the appropriate Ceramic [`peerlist`](https://github.com/ceramicnetwork/peerlist) file by submitting a pull request. This is used as a temporary measure to streamline peer discovery until `js-ipfs` supports DHT lookups.

## **Next steps**
Congratulations! You have now set up a hosted Ceramic node that is ready to receive HTTP requests from the [JS HTTP Client](../clients/javascript/http.md) or the [CLI](../clients/javascript/cli.md), or to simply serve as another node for redundant stream pinning and replication.

</br></br></br>
