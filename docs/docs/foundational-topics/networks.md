# Ceramic Networks

Since Ethereum is a protocol, this means there can be multiple independent "networks" conforming to this protocol that do not interact with each other.

Networks are different Ethereum environments you can access for development, testing, or production use cases. Your Ethereum account will work across the different networks but your account balance and transaction history wont carry over from the main Ethereum network. For testing purposes, it's useful to know which networks are available and how to get testnet ETH so you can play around with it.

Networks are collections of Ceramic [nodes](../glossary#nodes) that share specific configurations and communicate over dedicated [libp2p](../glossary#libp2p) topics. Networks are discrete from one another. [Streams](../glossary#streams) that exist on one network are _not_ discoverable on or portable to another.

The Ceramic network is a distributed, peer-to-peer network formed by Ceramic peers who participate in different ways.

Peers communicate over secure challens that they use to distribute information to the networrk (gossiping), to transfer data among themselves, and to discover other peers, maintaining a well-connected swarm in which information looks like blocks and messages flow swiftly even when many thousands of peers participate.

## Public Networks

---

Public networks are accessible to anyone in the world with an internet connection. Anyone can read or create transactions on a public blockchain and validate the transactions being executed. Agreement on transactions and the state of the network is decided by a consensus of peers.

Ceramic has three public networks that you can use when building applications: Mainnet, Clay Testnet, and Dev Unstable.

### Mainnet

Mainnet is the primary public Ethereum production blockchain, where actual-value transactions occur on the distributed ledger.

When people and exchanges discuss ETH prices, they're talking about Mainnet ETH.

Mainnet is the main public network used for production deployments on Ceramic. Ceramic's mainnet nodes communicate over the dedicated `/ceramic/mainnet` libp2p topic and use [Ethereum's](../glossary#ethereum) mainnet blockchain (`EIP155:1`) for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams). Mainnet is currently running and anyone can openly query data from streams on mainnet using the [community mainnet gateway](../run/nodes/community-nodes.md#gateways), but deployments that require writing data to the network are restricted to those projects on the mainnet early launch program (ELP) waitlist. If you want to write data to mainnet, [**sign up for ELP here**](https://blog.ceramic.network/ceramic-mainnet-early-launch-program/). _Over time, we will fully open mainnet to all applications._

### Testnets

In addition to Mainnet, there are public testnets. These are networks used by protocol developers or smart contract developers to test both protocol upgrades as well as potential smart contracts in a production-like environment before deployment to Mainnet. Think of this as an analog to production versus staging servers.

It’s generally important to test any contract code you write on a testnet before deploying to the Mainnet. If you're building a dapp that integrates with existing smart contracts, most projects have copies deployed to testnests that you can interact with.

Most testnets use a proof-of-authority consensus mechanism. This means a small number of nodes are chosen to validate transactions and create new blocks – staking their identity in the process. It's hard to incentivise mining on a proof-of-work testnet which can leave it vulnerable.

#### Clay

A proof-of-authority testnet that works across clients.

Clay Testnet is a public Ceramic network used by the community for application prototyping, development, and testing purposes. Ceramic core devs also use Clay for testing official protocol release candidates. While we aim to maintain a high level of quality on the Clay testnet that mirrors the expectations of Mainnet as closely as possible, ultimately the reliability, performance, and stability guarantees of the Clay network are lower than that of Mainnet. Because of this, **the Clay network should not be used for applications in production**. Clay nodes communicate over the dedicated `/ceramic/testnet-clay` libp2p topic and use [Ethereum's](../glossary#ethereum) Rinkeby and Ropsten testnet blockchains for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams). **Clay is openly available for anyone to use now.**

#### Dev-unstable

A proof-of-authority testnet for those running OpenEthereum clients.

Dev Unstable is a public Ceramic network used by Ceramic core protocol developers for testing new protocol features and the most recent commits on the develop branch of `js-ceramic`. It should be considered unstable and highly experimental; only use this network if you want to test the most cutting edge features, but expect issues. Dev Unstable nodes communicate over the dedicated `/ceramic/dev-unstable` libp2p topic and use [Ethereum's](../glossary#ethereum) Rinkeby and Ropsten testnet blockchains for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams).

#### Testnet faucets

ETH on testnets has no real value; therefore, there are no markets for testnet ETH. Since you need ETH to actually interact with Ethereum, most people get testnet ETH from faucets. Most faucets are webapps where you can input an address which you request ETH to be sent to.

Görli faucet
Kovan faucet
Rinkeby faucet
Ropsten faucet

## Private Networks

---

An Ethereum network is a private network if its nodes are not connected to a public network (i.e. Mainnet or a testnet). In this context, private only means reserved or isolated, rather than protected or secure.

You can also prototype applications on Ceramic by running the protocol in a local environment completely disconnected from other public nodes.

### Development networks

To develop an Ethereum application, you'll want to run it on a private network to see how it works before deploying it. Similar to how you create a local server on your computer for web development, you can create a local blockchain instance to test your dapp. This allows for much faster iteration than a public testnet.

There are projects and tools dedicated to assist with this. Learn more about development networks.

Local is a private test network used for the local development of Ceramic applications. Nodes connected to the same local network communicate over a randomly-generated libp2p topic `/ceramic/local-$(randomNumber)` and use a local Ethereum blockchain provided by Truffle's Ganache for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams).

### Consortium networks

The consensus process is controlled by a pre-defined set of nodes that are trusted. For example, a private network of known academic institutions that each govern a single node, and blocks are validated by a threshold of signatories within the network.

If a public Ethereum network is like the public internet, you can think of a consortium network as a private intranet.

## Further Reading

---

- Thing 1
- Thing 2
