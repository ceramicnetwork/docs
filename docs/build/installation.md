# Installation
Install a Ceramic client to perform [writes](./writes.md) and [queries](./queries.md) on the network.

!!! warning ""
    
    **Project status: Clay testnet is now live.**</br>`Clay` is a decentralized public network ready for experimental application development and testing, but you still may encounter a few issues.  It is the last major milestone before `Fire` mainnet, which is under development and will launch in late Q1 2021. Documents published on Clay will *not* be portable to Fire. Please [reach out on Discord](https://chat.ceramic.network) or [create an issue on Github](https://github.com/ceramicnetwork/js-ceramic/issues) to report any issues. Read the full announcement [here](https://blog.ceramic.network/ceramic-network-clay-testnet/){:target="_blank"}.


## **Clients**
Ceramic is available in a variety of clients suited for different use cases. For optimal performance, it is recommended that you use the HTTP Client when building an application.

Client | Description | Usage | Details |
| ------ | ----- | ---- | --- |
| HTTP | An API for interacting with a remote Ceramic daemon over HTTP | Runtime | [Learn](../reference/javascript/clients.md) |
| Core | An API for running the entire Ceramic protocol in a JavaScript environment | Runtime | [Learn](../reference/javascript/clients.md) |
| CLI | A command line interface for interacting with a Ceramic node, also used to spin up a Ceramic daemon | Development, Infrastructure | [Learn](../reference/javascript/clients.md) |

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
    
    !!! info "Node options"
        When using the HTTP API, you need to connect to a remote Ceramic node by passing its URL. Here are your options for nodes that run on the Clay testnet. Choose the option that best suits your use case:
        
        - Community gateway `https://gateway-clay.ceramic.network`: Provides read-only access to the Clay testnet. (recommended)
        - Community dev node `https://ceramic-clay.3boxlabs.com`: Provides write and read access to the Clay testnet. This node is occasionally wiped and does not guarantee document persistence. (recommended)
        - Run your own node `https://yourEndpoint.com`: Provides write and read access to the Clay testnet. Running your own node allows you to persist documents and have full control, however this is process is not yet well documented. If you choose to run your own node, be sure to add your node to the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json) by submitting a pull request. This allows other nodes to discover your node.
        - LocalHost `https://localhost:7007`: Provides read access to the Clay testnet. Writes made to this local node will only be available to nodes in the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json), but will not be available to other nodes on the network. Users need to first have a Ceramic daemon running locally using the CLI.

    #### Create an instance

    ``` javascript
    const ceramic = new CeramicClient(API_URL)
    ```

    #### Provide a DID instance to the Ceramic client.
    Ceramic instances need access to a DID instance to use to resolve DIDs and to create and validate cryptographic signatures on Ceramic commits. See the [Configure your DID](configure-did.md) page for more information on how to do this.

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

=== "CLI"

    #### Start the Ceramic daemon
    This commands starts a local Ceramic node.

    ```bash
    $ ceramic daemon
    ```

    #### Connect to a remote Ceramic node
    By default the Ceramic CLI communicates with the local node that you started with the `ceramic daemon` command. However, it is possible to use the CLI to communicate with a remote node. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

    ```bash
    $ ceramic config set ceramicHost 'https://yourceramicnode.com'
    ```

    !!! info "Node options"
        When using the CLI, you need to connect to a remote Ceramic node by passing its URL. Here are your options for nodes that run on the Clay testnet. Choose the option that best suits your use case:
        
        - LocalHost `https://localhost:7007`: Enabled by default in the CLI. Provides read access to the Clay testnet. Writes made to this local node will only be available to nodes in the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json), but will not be available to other nodes on the network.
        - Community gateway `https://gateway-clay.ceramic.network`: Provides read-only access to the Clay testnet.
        - Community dev node `https://ceramic-clay.3boxlabs.com`: Provides write and read access to the Clay testnet. This node is occasionally wiped and does not guarantee document persistence. (recommended)
        - Run your own node `https://yourEndpoint.com`: Provides write and read access to the Clay testnet. Running your own node allows you to persist documents and have full control, however this is process is not yet well documented. If you choose to run your own node, be sure to add your node to the [`peerlist`](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json) by submitting a pull request. This allows other nodes to discover your node.

</br>
</br>
</br>
