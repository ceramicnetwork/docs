# Installation
Install a Ceramic client to perform [transactions](./transactions.md) and [queries](./queries.md) on the network.

## Clients
Ceramic is available in a variety of clients suited for different use cases. For optimal performance, it is recommended that you use the HTTP Client when building an application.

Client | Description | Usage | Details |
| ------ | ----- | ---- | --- |
| HTTP | An API for interacting with a remote Ceramic daemon over HTTP | Runtime | [Learn](../reference/javascript/clients.md) |
| Core | An API for running the entire Ceramic protocol in a local JavaScript environment, such as directly in-browser | Runtime | [Learn](../reference/javascript/clients.md) |
| CLI | A command line interface for interacting with a Ceramic node | Development | [Learn](../reference/javascript/clients.md) |

## Installation
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

## Setup
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
    
    ??? note "Node options"
        When using the HTTP API, you need to connect to a remote Ceramic node by passing its URL. Here are your options for nodes that run on the Clay testnet. Choose the option that best suits your use case:
        
        - Community gateway `https://gateway-clay.ceramic.network`: Provides read-only access to the Clay testnet.
        - Community dev node `https://ceramic-clay.3boxlabs.com`: Provides write and read access to the Clay testnet. This node is periodically wiped and does not guarantee document persistence.
        - Run your own node `https://yourEndpoint.com`: Provides write and read access to the Clay testnet. Running your own node allows you to persist documents and have full control.
        - LocalHost `https://localhost:7007`: Provides write and read access to the Clay testnet. Users need to first have a Cermic daemon running locally using the CLI.

    #### Create an instance

    ``` javascript
    const ceramic = new CeramicClient(API_URL)
    ```

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
    import basicsImport from 'multiformats/cjs/src/basics-import.js'
    import legacy from 'multiformats/cjs/src/legacy.js'

    basicsImport.multicodec.add(dagJose)
    const format = legacy(basicsImport, dagJose.name)
    ```

    #### Create an IPFS instance

    ``` javascript
    const ipfs = Ipfs.create({
        ipld: { formats: [format] },
    })
    ```

    #### Create a Ceramic instance
    Create an instance of Ceramic by passing ipfs and an optional configuration
    object.

    ``` javascript
    const ceramic = await Ceramic.create(ipfs, config)
    ```

=== "CLI"

    #### Start the Ceramic daemon
    This commands starts a local Ceramic node.

    ```bash
    $ ceramic daemon
    ```

    #### Connect to a remote Ceramic node
    By default the Ceramic CLI communicates with the local node that you started with the `ceramic daemon` command. However, it is possible to use the CLI to communicate with a remote node. To do this, use the `config set` command to set the `ceramicHost` variable to the URL of the node you wish to use.

    ```bash
    $ ceramic config set ceramicHost 'https://ceramic-clay-gateway.3boxlabs.com'
    ```

    ??? note "Node options"
        When using the CLI, you need to connect to a remote Ceramic node by passing its URL. Here are your options for nodes that run on the Clay testnet. Choose the option that best suits your use case:
        
        - LocalHost `https://localhost:7007`: Enabled by default in the CLI. Provides write and read access to the Clay testnet.
        - Community gateway `https://gateway-clay.ceramic.network`: Provides read-only access to the Clay testnet.
        - Community dev node `https://ceramic-clay.3boxlabs.com`: Provides write and read access to the Clay testnet. This node is periodically wiped and does not guarantee document persistence.
        - Run your own node `https://yourEndpoint.com`: Provides write and read access to the Clay testnet. Running your own node allows you to persist documents and have full control.

</br>
</br>
</br>
