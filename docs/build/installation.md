# Installation
Install a Ceramic client to interact with the network

## Clients
Ceramic is available in a variety of clients suited for different use cases.

Client | Description | Usage | Details |
| ------ | ----- | ---- | --- |
| HTTP | An API for interacting with a remote Ceramic daemon over HTTP | Application runtime | [Learn]() |
| Core | An API for interacting with a Ceramic node in a local JavaScript environment, such as directly in-browser | Application runtime | [Learn]() |
| CLI | A command line interface for interacting with a Ceramic node | Development time | [Learn]() |

## Installation
Open your terminal and install a client using [npm]().

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

    The CLI requires the use of [Node.js](). Make sure to have an up-to-date
    version installed on your machine.

## Setup
Setup your client within your project.

=== "HTTP"

    #### Import the HTTP client

    ``` javascript
    import CeramicClient from '@ceramicnetwork/http-client'
    ```

    #### Connect to a node
    If you are running the `ceramic daemon` command locally you can use
    localhost and port 7007.

    ``` javascript
    const API_URL = "http://localhost:7007"
    ```

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
    IPLD codec to store signed and encrypted data. In order to create an
    instance of Cerami core, you first need to create an instance of *js-ipfs*
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
    Create an instance of ceramic by passing ipfs and an optional configuration
    object.

    ``` javascript
    const ceramic = await Ceramic.create(ipfs, config)
    ```

=== "CLI"

    #### Start the Ceramic daemon

    ```bash
    ceramic daemon
    ```

    #### Connect to a Ceramic node (optional)

    ```bash
    ceramic daemon
    ```

    ??? tip "Node options"
        By default the CLI will start a Ceramic node on your local machine and
        connect to it on port 7007, `http://localhost:7007`. If you would like
        to use another node, the community offers various nodes:

        - [Read-only gateway]()
        - [Dev node]()

        Or you could [run a node]() and connect to it.

## Examples

TODO - what do we want to do with these examples?

Your project should look something like this. You may have slight modifications
depending on your configuration.

=== "HTTP"

    ``` javascript
    import CeramicClient from '@ceramicnetwork/http-client'
    ```

=== "Core"

    ``` javascript
    import Ceramic from '@ceramicnetwork/core'
    ```

=== "CLI"

    ```bash
    ceramic daemon
    ```

</br>
</br>
