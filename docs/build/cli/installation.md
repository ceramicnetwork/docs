# CLI Installation

The CLI provides a way to start a JavaScript Ceramic node and interact with it from the command line. This page describes how to set up your CLI.

!!! warning ""

    If you are looking to spin up a hosted node for other use cases such as for use with the [JS HTTP Client](../javascript/http.md) or as a secondary node for redundant stream pinning and replication, see [Hosting a node](../../run/nodes/nodes.md).

## **Requirements**

Installing the CLI requires a console, [Node.js](https://nodejs.org/en/){:target="\_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="\_blank"} v6. Make sure to have these installed on your machine.

!!! warning ""

    While npm v7 is not officially supported, you may still be able to get it to work. You will need to install the `node-pre-gyp` package globally. This is required until `node-webrtc` which IPFS depends on [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694){:target="_blank"}.

    ```bash
    npm install -g node-pre-gyp
    ```

!!! warning "Depreciation Notice:"

    Interactions with Streams and TileDocuments have been moved to our [Glaze CLI](../../../tools/glaze/development#cli). If you encounter any errors with the commands below please retry using the Glaze CLI first.

## **1. Install the CLI**

Open your console and install the CLI using npm.

```bash
npm install -g @ceramicnetwork/cli
```

## **2. Start the Ceramic node**

This starts a local JavaScript Ceramic node on the [Clay Testnet](../../learn/networks.md#clay-testnet) at `https://localhost:7007`.

```bash
ceramic daemon
```

This `localhost` setup allows you to read streams from other nodes connected on the same [network](../../learn/networks.md), but writes to your local node will only be available on your local node and on other nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json). They will not be available to every node on the network. For greater connectivity, you might want to connect your CLI to a remote long-lived Ceramic node.

## **3. Configure a network**

(Optional) By default, the JS CLI starts a node on the [Clay Testnet](../../learn/networks.md#clay-testnet). If you would like to use a different network, you can specify this using the `--network` option. View [available networks](../../learn/networks.md). Note, the CLI can not yet be used with [Mainnet](../../learn/networks.md#mainnet).

## **4. Configure a node URL**

(Optional) It is possible to use the CLI with a remote Ceramic node over HTTP, instead of a local node. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

```bash
ceramic config set ceramicHost 'https://yourceramicnode.com'
```

When using the CLI with a remote node, you have a few options:

- [Free community nodes](../../run/nodes/community-nodes.md)
- [Commercial node providers](../../run/nodes/node-providers.md)
- [Host your own node](../../run/nodes/nodes.md)

## **Authentication**

By default, the CLI is authenticated using the [Key DID Provider](../../authentication/key-did/provider.md). The seed for this DID is stored in `~/.ceramic/config.json`. If this file is not present on startup a new DID will be randomly generated. It's currently not possible to use the Ceramic CLI with other DID methods.
