# Node configuration
There are multiple ways to setup, run, and use Ceramic nodes and the optimal setup for you will differ based on you and your users needs.

## Core
Ceramic core contains the full implementation of the Ceramic protocol and can be used directly through the JavaScript *CeramicApi*. The core implementation can be used in any JavaScript environment, such as your tests, in-browser, or nodejs. When running Ceramic core all streams that are created, loaded, or pinned get verified in the local environment, which is great if you need maximal decentralization and resilience in your application. However, this great power comes with responsibility. In order to run Ceramic Core you need to configure your own IPFS node which supports *dag-jose* and ensure connectivity to the rest of the Ceramic network. Further, a stream created on an instance of Ceramic core will only be available on the network as long as this node remains online. So if your setup for example uses in-browser nodes and your user closes the tab, any stream created by that user will remain unavailable until the user opens the application again. One way to mitigate this is to pin streams on separate long running nodes.

## CLI
The Ceramic command line interface provides a way to spawn a Ceramic daemon and to interact with this (or remote) daemon though simple commands.

### Ceramic daemon
The simplest way to run a Ceramic node is to run the `ceramic daemon` command in your console. This command starts a Ceramic node with a standard cofiguration in a few steps. First a properly configured IPFS instance is started, IPFS is then used to create an instance of Ceramic core. Once Ceramic core is running an HTTP server is started that serves the [Ceramic HTTP API](./reference/http-api.md). The node is now ready to accept requests from the local environment or remotely.

By default the daemon runs a bundled IPFS instance. It's also possible to run a separate service for IPFS using the `@ceramicnetwork/ipfs-daemon` npm package. Regardless of how the IPFS instance is managed, if you want your node to be discoverable from the network, make sure the appropriate ports used by IPFS and the Ceramic HTTP API are publicly accessible (for example if you are behind a router you may need to set up port forwarding). Also be sure to add your node to the [peerlist](https://github.com/ceramicnetwork/peerlist/blob/main/testnet-clay.json) by submitting a pull request (this is a temporary measure to enhance peer discovery since js-ipfs doesn't support DHT lookups yet).


## HTTP Client
The Ceramic HTTP client is a lighter way of interacting with the Ceramic network. It connects to a remote Ceramic http endpoint to read and write data. Important to note however is the user's keys always live client side and all updates are authored on the client and then sent to the remote http endpoint to write the update to the network. The HTTP client also implements the *CeramicApi* so it can be used interchangeably with Core.

## Hosted nodes
The main consideration when using the Ceramic HTTP client is which remote Ceramic node to use. Options include running your own node, or using a node managed by some service provider.

## Hosting your own node
To get started running your own node you can basically just run the `ceramic daemon` command. This will get you started with a simple node setup. When running a node in production there are a variety of factors to consider, but we won't go into details here. After starting your node it should be available on `http://localhost:7007`.

## Using the gateway
If you only want to read data from the network you can use the community gateway. This is a node hosted by 3Box Labs which have stream writes disabled.

* `https://gateway.ceramic.network` - the gateway for Ceramic mainnet
* `https://gateway-clay.ceramic.network` - the gateway for the Clay testnet

## 3Box Labs test node
3Box Labs provides a node that can be used to get started developing on the Clay testnet. This node will run the latest *release candidate* of the Ceramic protocol. The node will be periodically wiped, so don't rely on it for production data (you shouldn't anyway since it's a testnet).

* `https://ceramic-clay.3boxlabs.com`


## Third party service providers

Contact 3Box Labs if you want to be listed here!
