# JS CLI Client
The JS CLI allows you to start a JavaScript Ceramic node and interact with it using simple commands. It can be used to interact with Ceramic from the command line, or to simply spin up and configure a Ceramic node which can be used with, for example, the [JS HTTP Client](./http.md).

## **Installation**

Installing the JS CLI requires a console, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

!!! warning ""

    While npm v7 is not officially supported, you may still be able to get it to work. You will need to install the `node-pre-gyp` package globally. This is required until `node-webrtc` which IPFS depends on [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694){:target="_blank"}.
    
    ```bash
    npm install -g node-pre-gyp
    ```

### 1. Install the CLI
Open your console and install the JS CLI using npm.

``` bash
npm install -g @ceramicnetwork/cli
```

### 2. Start the Ceramic node
This starts a local Ceramic node on the [Clay Testnet](../../learn/networks.md#clay-testnet) at `https://localhost:7007`. 

```bash
$ ceramic daemon
```

This localhost setup allows you to read streams from other nodes connected on the same network, but writes to your local node will only be available on your local node and on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json). They will not be available to every node on the network. For greater connectivity, follow the step below to connect your CLI to a remote long-lived Ceramic node.

### 3. Configure a network (optional)
By default, the JS CLI starts a node on the [Clay Testnet](../../learn/networks.md#clay-testnet). If you would like to use a different network, you can specify this using the `--network` option. View [available networks](../../learn/networks.md).

### 4. Configure a node URL (optional)
It is possible to use the CLI with a remote Ceramic node over HTTP. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

```bash
$ ceramic config set ceramicHost 'https://yourceramicnode.com'
```

When using the CLI with a remote node, you have a few options:

- [Free community nodes](../../tools/hosted-nodes/community-nodes.md)
- [Commercial node providers](../../tools/hosted-nodes/node-providers.md)
- [Host your own node](../../run/nodes.md)

## **Authentication**
By default, the CLI is authenticated using the [Key DID Provider](https://github.com/ceramicnetwork/key-did-provider-ed25519){:target="_blank"}. The seed for this DID is stored in `~/.ceramic/config.json`. If this file is not present on startup a new DID will be randomly generated. It's currently not possible to use the Ceramic CLI with other DID methods.

## **Usage**
Use the `ceramic daemon -h` command to see additional commands and options. To try basic functionality using the CLI, visit the [Quick Start](../../build/quick-start.md) guide.


</br></br></br>
