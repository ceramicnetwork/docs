# Nodes and clients

Ceramic is a distributed network of computers running software (nodes) that verify and replicate transactions and store streams. You need an application, known as a client, on your computer to "run" a node.

## Prerequisites

---

You should understand the concept of a peer-to-peer network and the basics of the EVM before diving deeper and running your own instance of a Ceramic client. Take a look at our introduction to Ceramic.

## What are nodes and clients?

---

"Node" refers to a running piece of client software. A client is an implementation of Ceramic that verifies transactions, keeping the network secure and the data accurate.

JS Ceramic is the Ceramic client that is the most advanced and feature complete. It is written in JavaScript. Other client implementations in other languages may be developed by the community. What these implementations have in common is they all follow a formal specification. This specification dictates how the Ceramic network functions.

> Want to work on a new client? Create a PR in this repo and reach out on Discord to share your ideas with the community.

Ceramic nodes are peers that run a full implementation of the Ceramic protocol. Their responsibility is to sync data with other peers on the network, validate transactions, maintain state, and store data.

Ceramic nodes can also publish different types of messages to the network by broadcasting them. For example, a node can publish a message to apply a transaction to a stream. Nodes can initiate transactions and queries to other Ceramic nodes.

Running a Ceramic node is a low-level task that usually implies keeping a program running 24/7. There are several Ceramic node implementations in the works, with Graphite being the most advanced.

## Why should I run a Ceramic node?

---

Running a node allows you to trustlessly and privately use Ceramic while supporting the ecosystem.

### Benefits to you

- Running your own node enables you to use Ceramic in a truly private, self-sufficient and trustless manner. You don't need to trust the network because you can verify the data yourself with your client. "Don't trust, verify" is a popular blockchain mantra.
- Your node verifies all the transactions and blocks against consensus rules by itself. This means you don’t have to rely on any other nodes in the network or fully trust them.
- You won't have to leak your addresses to random nodes. Everything can be checked with your own client.
- You have more control over your dapp's data
- How you access Ethereum via your application and nodes

### Network benefits

- A diverse set of nodes is important for Ceramic's health, security and operational resiliency.
- More replicaton and data redundancy
- Decentralization

## Running your own node

---

Interested in running your own Ceramic client? Learn how to spin up your own Ceramic node by following one of these guides:

<div class="txtl-options center">
  <a href="./hub/" class="box">
    <h4>Launch and manage a JS Ceramic node ↗</h4>
  </a>
</div>

## Alternatives to running your own node

---

Running your own node can be time consuming and you don’t always need to run your own instance of Ceramic. In this case, you can use a third party API provider like Infura, Alchemy, or QuikNode. Alternatively ArchiveNode is a community-funded Archive node that hopes to bring archive data on the Ethereum blockchain to independent developers who otherwise couldn't afford it. For an overview of using these services, check out nodes as a services.

If somebody runs an Ethereum node with a public API in your community, you can point your light wallets (like MetaMask) to a community node via Custom RPC and gain more privacy than with some random trusted third party.

On the other hand, if you run a client, you can share it with your friends who might need it.

##### Community Nodes

Here is a list of free, community-operated Ceramic nodes that you can use to to quickly prototype your application before launching your own node.

| Network | Access      | Node software | URL      | Maintainer |
| ----- | ----------- | ----------- | ----------- | ----------- |
| Clay testnet | Read/Write      | JS Ceramic       | `https://ceramic-clay.3boxlabs.com`      | 3Box Labs       |
| Clay testnet | Read-only   | JS Ceramic        | `https://gateway-clay.ceramic.network`   | 3Box Labs   |
| Mainnet | Read/Write   | JS Ceramic        | Request Access   | 3Box Labs        |
| Mainnet | Read-only   | JS Ceramic        | `https://gateway.ceramic.network`   | 3Box Labs        |

!!! warning

    These nodes are only meant to help you get started. You **should not** rely on them for long-term data availability or production usage. The storage for these nodes will be periodically wiped. If your streams have not been replicated by another node when this happens they will be lost. If you need long-term data persistence or production usage, consider [running your own node](./nodes.md).

## Clients

---

The Ceramic community currently maintains one fully-featured client, JS Ceramic, which is implemented in JavaScript, is fully open-source, and developed primarily by the 3Box Labs team with support and contributions from the community.

| Client | Language | Operating systems | Networks |
| --- | --- | --- | --- |
| [JS Ceramic ↗]() | JavaScript | Linux, Windows, macOS | Mainnet, Clay, Dev-Unstable |



## About JS Ceramic

---

JS Ceramic is the original implementation of the Ceramic protocol. Currently, it is the most widespread client with the biggest user base and variety of tooling for users and developers. It is written in JavaScript, fully open source, and licensed under MIT and Apache 2.


## Hardware Requirements

---

Hardware requirements differ by client but generally are not that high since the node just needs to stay synced. Sync time and performance do improve with more powerful hardware however. Depending on your needs and wants, Ceramic can be run on your computer, home server, single-board computers or virtual private servers in the cloud.

Before installing any client, please ensure your computer has enough resources to run it. Minimum and recommended requirements can be found below, however the key part is memory. Syncing the Ethereum blockchain is very input/output intensive. It is best to have a solid-state drive (SSD). To run an Ethereum client on HDD, you will need at least 8GB of RAM to use as a cache.

##### Minimum requirements

- CPU with 2+ cores
- 4 GB RAM minimum with an SSD, 8 GB+ if you have an HDD
- 8 MBit/s bandwidth

##### Recommended specifications

- Fast CPU with 4+ cores
- 16 GB+ RAM
- Fast SSD with at least 500 GB free space
- 25+ MBit/s bandwidth
The sync mode you choose will affect space requirements but we've estimated the disk space you'll need for each client below.

## Further Reading

---

There is a lot of instructions and information about Ethereum clients on the internet, here are few that might be helpful.

- Ethereum 101 - Part 2 - Understanding Nodes – Wil Barnes, 13 February 2019
- Running Ethereum Full Nodes: A Guide for the Barely Motivated – Justin Leroux, 7 November 2019
- Running an Ethereum Node – ETHHub, updated often
- Analyzing the hardware requirements to be an Ethereum full validated node – Albert Palau, 24 September 2018
- Running a Hyperledger Besu Node on the Ethereum Mainnet: Benefits, Requirements, and Setup – Felipe Faraggi, 7 May 2020

## Related Topics

---

- Blocks
- Networks

## Related Tutorials

---

- Running a Node with Geth – How to download, install and run Geth. Covering syncmodes, the Javascript console, and more.
- Turn your Raspberry Pi 4 into an Eth 1.0 or Eth 2.0 node just by flashing the MicroSD card – Installation guide – Flash your Raspberry Pi 4, plug in an ethernet cable, connect the SSD disk and power up the device to turn the Raspberry Pi 4 into a full Ethereum 1.0 node or an Ethereum 2.0 node (beacon chain / validator).