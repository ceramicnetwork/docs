# Running a node
This guide describes how to run a hosted Ceramic node that can be used by a Ceramic client to interact with the Ceramic network. Currently, it is only possible to run nodes on the Clay testnet. We will add the ability to run nodes on mainnet in the near future.


## **Prerequisites**

This installation guide will use a terminal, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
```bash
npm install -g node-pre-gyp
```
This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).


## **Installation**

Open your console and install the CLI.

``` bash
$ npm install -g @ceramicnetwork/cli
```


## **Start the daemon**

Next, start the Ceramic daemon.

```bash
$ ceramic daemon
```

This command starts a Ceramic node with a standard cofiguration. By default the daemon runs a bundled IPFS instance. First, a properly configured IPFS instance is started. Then, IPFS is used to create an instance of Ceramic core. Once Ceramic core is running, an HTTP server is started that serves the Ceramic HTTP API. The node is now ready to accept requests from the local environment or remotely.

### Using an external IPFS node

It's also possible to run a separate service for IPFS using the `@ceramicnetwork/ipfs-daemon` npm package. 


## **Network connectivity**

### Expose ports

If you want your node to be discoverable to the rest of the Ceramic Network, make sure the appropriate ports used by IPFS and the Ceramic HTTP API (by default this is port 7007) are publicly accessible. For example, if you are behind a router you may need to set up port forwarding. 

### Enable peer discovery

To enable peer discovery, add your node to the appropriate Ceramic [`peerlist`](https://github.com/ceramicnetwork/peerlist) file by submitting a [pull request](https://github.com/ceramicnetwork/peerlist). Note: this is a temporary measure to streamline peer discovery since `js-ipfs` doesn't yet support DHT lookups.
