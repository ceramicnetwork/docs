# JS CLI Client
The JS CLI allows you to start a JavaScript Ceramic node and interact with it using simple commands. It can be used to interact with Ceramic from the command line, or to simply spin up and configure a Ceramic node which can be used with, for example, the JS HTTP Client.

## **Considerations**

**Simplified development**:

**Spin up a Ceramic node**:

## **Installation**

Installing the JS CLI requires a terminal, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

    !!! warning ""
    While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
    
    ```bash
    npm install -g node-pre-gyp
    ```
    
    *This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).*

### 1. Install the CLI
Open your terminal and install the JS CLI using npm.

``` bash
npm install -g @ceramicnetwork/cli
```

### 2. Start the Ceramic node
This command starts a local Ceramic node at `https://localhost:7007`. 

```bash
$ ceramic daemon
```

This localhost setup allows you to read streams from other nodes connected on the Ceramic network, but writes to your local node will only be available on your local node and on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json), but will not be available to every node on the network. For greater connectivity, follow the next step to connect your CLI to a remote long-lived Ceramic node.

### 3. Configure a node URL (optional)
It is possible to use the CLI with a remote Ceramic node over HTTP. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

```bash
$ ceramic config set ceramicHost 'https://yourceramicnode.com'
```

When using the CLI with a remote node, you have a few options:

- [Free community nodes](../../tools/hosted-nodes/community-nodes.md)
- [Commercial node providers](../../tools/hosted-nodes/node-providers.md)
- [Host your own node](../../run/nodes.md)

## **Authentication**
The JS CLI comes prepacked with the [Key DID Provider]() for authenticating and writing data to streams. At this time, no other DID Provider can be used with the CLI.

## **Usage**
Type `--h` for a full list of CLI commands.

</br></br></br>
