# Hosting a node

This guide describes how to run a Ceramic node that can be used as a remote node by the [JS HTTP Client](../../build/javascript/installation.md#js-http-client) or the [CLI](../../build/cli/installation.md#4-configure-a-node-url). It is best to start by connecting to the Clay testnet, however, you do not need to run your own node to use testnet. You can get started with Ceramic right away by using a community node: [https://developers.ceramic.network/run/nodes/community-nodes/](https://developers.ceramic.network/run/nodes/community-nodes/)

When you are ready to get on mainnet, reach out to the 3Box team on Discord: [https://discord.gg/tsQXsG8Sde](https://discord.gg/tsQXsG8Sde). For now, mainnet is currently limited to early partners.

---

## Getting Started

The following is an overview of the steps you must take to run a Ceramic node. Details for each step are described in this document.

1.  Running the Daemon

    Determine if you want to run IPFS in-process or [out-of-process](#ipfs-out-of-process).

2.  Data Persistence

    Determine your mechanism for data persistence and ensuring your IPFS multiaddress will not change.

    !!! warning ""

        Data persistence is the most critical step to properly run a Ceramic node. It is critical to persist the multiaddress for network connectivity, and the Ceramic State Store and IPFS Repo to persist stream data, as there are currently no guarantees that another node is keeping a copy of the data. 

3.  Resource Allocation and Networking

    Run your Ceramic node and IPFS with data persistence and networking configured.

4.  Staying Connected

    Submit a pull request to the [Ceramic peerlist](https://github.com/ceramicnetwork/peerlist) with the multiaddress of your IPFS node, the IP address for your Ceramic node, and a description of the data persistence setup for the multiaddress, Ceramic State Store and IPFS Repo. Once your pull request is merged in you will be connected to the Ceramic network and the [Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service).
    
    !!! info ""
 

        Mainnet nodes will not run immediately after start up until your IP address is added to the allow list for the 3Box hosted anchor service and your PR to the peerlist is merged.



## Networking

### Ceramic

The Ceramic daemon serves an API that you will use to interact with your Ceramic node. The default API port is `7007`. Make sure this port is available to whatever clients you plan to use.

!!! info ""
Healthchecks can be run against the API endpoint `/api/v0/node/healthcheck`.

### IPFS

Your Ceramic node connects to the Ceramic network by using IPFS. IPFS nodes connect to each other using a Libp2p module called "switch" (aka "swarm"). This module operates over a websocket, on port `4011` by default. The websocket port must be accessible by the internet so your Ceramic node can be connected to the network.

!!! info ""
We recommend using SSL for a secure websocket (port `4012` by default).

!!! info ""
Healthchecks can be run against the `HEALTHCHECK_PORT` (port `8011` by default) when `HEALTHCHECK_ENABLED` is `true`.

#### **Out-of-process**

When running IPFS out-of-process, its API port must be accessible by the Ceramic node. The default API port is `5011`. The IPFS node address will then be passed to Ceramic with a CLI flag `--ipfs-api <ipfs_api_url>`.

## Staying Connected

The ipfs-daemon designed for use with Ceramic has the IPFS node discovery mechanism, Libp2p DHT, turned off. We have configured this by default because the JavaScript implementation of the DHT is not yet reliable. With node discovery turned off, we must manually create a connected network of peers by sharing known addresses and dialing them explicitly. The ipfs-daemon package handles this logic and requires that every node that wants to be in the network be in a "peerlist" which is maintained here: [https://github.com/ceramicnetwork/peerlist](https://github.com/ceramicnetwork/peerlist)

#### Peerlist

Once you have fully configured your Ceramic node with this guide and have a way to persist its configuration and state, submit a pull request to the peerlist with the multiaddress of your IPFS node, the IP address for your Ceramic node, and a brief description of your data persistence setup for the Ceramic State Store and IPFS Repo. When a pull request is submitted, it triggers a connectivity test to ensure the node can successfully connect to the network. If this fails, the 3Box Labs team will reach out to you directly to triage the issue. Make sure there are no firewalls blocking your instance and that your port is properly exposed. Once your multiaddress is added, you will be able to stay connected to other nodes in the network.

Once you are on the peerlist, you should monitor your IPFS node and alert our team on Discord in the case of any planned or unexpected downtime. Please make your best effort to come back online within 24 hours. If we can not connect to your IPFS node for over 24 hours, we will remove it from the peerlist and you can resubmit your multiaddress in a new PR once your node becomes stable again. If the connectivity test in your PR to the peerlist fails and it is due to a node other than your own, we will update the peerlist and re-run the tests for you.


## **Next steps**

Congratulations! You have now set up a hosted Ceramic node that is ready to receive HTTP requests from the local environment, the [JS HTTP Client](../../build/javascript/installation.md#js-http-client), the [CLI](../../build/cli/installation.md#4-configure-a-node-url), or to simply serve as another node to replicate and pin streams. Please report any bugs as issues at [https://github.com/ceramicnetwork/js-ceramic](https://github.com/ceramicnetwork/js-ceramic).
