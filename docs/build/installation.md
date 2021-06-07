# Installation
Install a [Ceramic client](../learn/clients.md) to perform [writes](./writes.md) and [queries](./queries.md) on the network.

!!! warning ""
    
    **Project status: Clay testnet is now live.**</br>`Clay` is a decentralized public network ready for experimental application development and testing, but you still may encounter a few issues.  It is the last major milestone before `Fire` mainnet, which is under development and will launch in late Q1 2021. Documents published on Clay will *not* be portable to Fire. Please [reach out on Discord](https://chat.ceramic.network) or [create an issue on Github](https://github.com/ceramicnetwork/js-ceramic/issues) to report any issues. Read the full announcement [here](https://blog.ceramic.network/ceramic-network-clay-testnet/){:target="_blank"}.


## **Clients**
Ceramic is available in a variety of clients suited for different use cases. For optimal performance and data availability, it is recommended that you use the HTTP Client when building an application. For a full overview of the available Ceramic clients and their tradeoffs, see [Clients](../learn/clients.md).

Client | Description | Usage | Details |
| ------ | ----- | ---- | --- |
| HTTP | An API for interacting with a remote Ceramic daemon over HTTP | Runtime | [Learn](../learn/clients.md/#http-client) |
| Core | An API for running the entire Ceramic protocol in a JavaScript environment | Runtime | [Learn](../learn/clients.md/#core) |
| CLI | A command line interface for interacting with a Ceramic node, also used to spin up a Ceramic daemon | Development, Infrastructure | [Learn](../learn/clients.md/#cli) |

## **Prerequisites**

This installation guide will use a terminal, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
```bash
npm install -g node-pre-gyp
```
This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).

## **Installation**
Open your terminal and install a client using npm.

=== "HTTP"

    ``` bash
    $ npm install @ceramicnetwork/http-client
    ```

=== "Core"

    ``` bash
    $ npm install @ceramicnetwork/core
    ```

=== "CLI"

    ``` bash
    $ npm install -g @ceramicnetwork/cli
    ```

    The CLI requires Node.js. Make sure to have an up-to-date version installed on your machine.

## **Setup**
Setup your client within your project.

=== "HTTP"

    #### Import the HTTP client

    ``` javascript
    import CeramicClient from '@ceramicnetwork/http-client'
    ```

    #### Connect to a node

    ``` javascript
    const API_URL = "http://yourceramicnode.com"
    ```
    
    When using the HTTP API, you need to connect to a remote Ceramic node by passing its URL. For this, you have a few options:
    - [Free community nodes](../tools/hosted-nodes/community-nodes.md)
    - [Third-party hosting services](../tools/hosted-nodes/node-providers.md)
    - [Run your own node](../run/nodes.md)
    - LocalHost: To use the HTTP Client with a Ceramic daemon running on your local machine, first follow the steps on this page for installing the CLI. Once the CLI is installed, you can pass `https://localhost:7007` to the HTTP client. Please note that this setup will allow you to read streams from other nodes connected on the Ceramic network, but writes to your local node will only be available on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json), but will not be available to other nodes on the network.

    #### Create an instance

    ``` javascript
    const ceramic = new CeramicClient(API_URL)
    ```

    #### Authenticate your client to perform writes
    Ceramic clients need access to a DID instance to create and validate cryptographic signatures on Ceramic commits. If you only intend to load streams you can skip this step. If you want to be able to create and update streams, however, you'll need to [Configure a DID](configure-did.md) and then [Authenticate](authentication.md) it.

=== "Core"

    #### Import the Core client

    ``` javascript
    import Ceramic from '@ceramicnetwork/core'
    ```

    #### Import IPFS with dag-jose
    Ceramic utilizes the [dag-jose](https://github.com/ipld/specs/blob/master/block-layer/codecs/dag-jose.md){:target="_blank"}
    IPLD codec to store signed and encrypted data in IPFS. In order to create an
    instance of Ceramic core, you first need to create an instance of *js-ipfs*
    with *dag-jose* enabled.

    ``` javascript
    import IPFS from 'ipfs'
    import dagJose from 'dag-jose'
    import { sha256 } from 'multiformats/hashes/sha2'
    import legacy from 'multiformats/legacy'

    const hasher = {}
    hasher[sha256.code] = sha256
    const dagJoseFormat = legacy(dagJose, {hashes: hasher})

    ```

    #### Create an IPFS instance

    ``` javascript
    const ipfs = await Ipfs.create({ ipld: { formats: [dagJoseFormat] } })
    ```

    #### Create a Ceramic instance
    Create an instance of Ceramic by passing ipfs and an optional configuration
    object.

    ``` javascript
    const ceramic = await Ceramic.create(ipfs, config)
    ```

    #### Provide a DID instance to the Ceramic client.
    Ceramic instances need access to a DID instance to use to resolve DIDs and to create and validate cryptographic signatures on Ceramic commits. See the [Configure your DID](configure-did.md) page for more information on how to do this.

    #### Authenticate your client to perform writes
    Ceramic instances need a DID Provider to create cryptographic signatures on Ceramic commits. If you only intend to load streams you can skip this step. If you want to be able to create and update streams, however, you'll need to authenticate your client. See the [Authentication](authentication.md) page for more information on how to do this.


=== "CLI"

    #### Start the Ceramic daemon
    This commands starts a local Ceramic node at `https://localhost:7007`. 

    ```bash
    $ ceramic daemon
    ```
    
    Please note that this setup allows you to read streams from other nodes connected on the Ceramic network, but writes to your local node will only be available on your local node and on nodes found on the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json), but will not be available to every node on the network. For greater connectivity, connect your CLI to a long-lived remote Ceramic node.

    #### Connect to a remote Ceramic node
    It is possible to use the CLI with a remote Ceramic node. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

    ```bash
    $ ceramic config set ceramicHost 'https://yourceramicnode.com'
    ```
    
    When using the CLI with a remote node, you have a few options:
    - [Free community nodes](../tools/hosted-nodes/community-nodes.md)
    - [Third-party hosting services](../tools/hosted-nodes/node-providers.md)
    - [Run your own node](../run/nodes.md)

</br>
</br>
</br>
