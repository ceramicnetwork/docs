# Ceramic Clients
There are multiple ways to setup, run, and use Ceramic. The optimal setup will differ based on your application's and your users' needs. This page describes the various options.

- [Core Client](#core)
- [CLI](#cli)
- [HTTP Client](#http-client)

To install one of these clients, see [Installation](../build/installation.md).


## **Core Client**
Ceramic core contains the full implementation of the Ceramic protocol and can be used directly through the JavaScript *CeramicApi*. The core implementation can be used in any JavaScript environment, such as your tests, in-browser, or nodejs. When running Ceramic core all streams that are created, loaded, or pinned get verified in the local environment, which is great if you need maximal decentralization and resilience in your application. However, this great power comes with responsibility. In order to run Ceramic Core you need to configure your own IPFS node which supports *dag-jose* and ensure connectivity to the rest of the Ceramic network. Further, a stream created on an instance of Ceramic core will only be available on the network as long as this node remains online. So if your setup for example uses in-browser nodes and your user closes the tab, any stream created by that user will remain unavailable until the user opens the application again. One way to mitigate this is to pin streams on separate long running nodes.

> To install the Core client, see [Installation](../build/installation.md).


## **CLI Client**
The Ceramic command line interface provides a way to spawn a Ceramic daemon and to interact with this (or remote) daemon though simple commands.

> To install the CLI, see [Installation](../build/installation.md).


## **HTTP Client**
The Ceramic HTTP client is a lighter way of interacting with the Ceramic network. It connects to a remote Ceramic http endpoint to read and write data. This means that state validation happens in the remote node which the client trusts. Important to note however is the user's keys always live client side and all updates are authored on the client and then sent to the remote http endpoint to write the update to the network. The HTTP client also implements the *CeramicApi* so it can be used interchangeably with Core.

The main consideration when using the Ceramic HTTP client is which remote Ceramic node to use. Options include [running your own node](../run/nodes.md) or using a node managed by some third-party hosting service.

> To install the HTTP client, see [Installation](../build/installation.md).

### Self-hosted nodes
To run your own node, see [Running a node](../run/nodes.md).

### Managed nodes
The Ceramic community offers free hosted nodes for use during development. See [Community Nodes](../tools/hosted-nodes/community-nodes.md) for the HTTP endpoints of these nodes. In the future, there will be third-party services that offer commercial-grade nodes for development. Once available, you can find the list [here](../tools/hosted-nodes/node-providers.md).

