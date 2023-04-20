# Networks

Information about the default Ceramic networks

## Overview
---

Networks are collections of Ceramic [nodes](../nodes.md) that share specific configurations and communicate over dedicated [libp2p](https://libp2p.io/) pubsub topics. Networks are disjoint from one another; streamsthat exist on one network are **not** discoverable or usable on another.

These pubsub topics are used to relay all messages for the defined networking sub protocols, including [Tip Gossip](tip-gossip.md) and [Tip Queries](tip-queries.md). 

## All Networks
---

An overview of the various Ceramic networks available today:

| Name | Network ID | Ceramic Pubsub Topic | Timestamp Authority | Type |
| --- | --- | --- | --- | --- |
| Mainnet | mainnet | /ceramic/mainnet | Ethereum Mainnet (EIP155:1) | Public |
| Clay Testnet | testnet-clay | /ceramic/testnet-clay | Ethereum Gnosis Chain | Public |
| Dev Unstable | dev-unstable | /ceramic/dev-unstable | Ethereum Goerli Testnet | Public |
| Local | local | /ceramic/local-$(randomNumber) | Ethereum by Truffle Ganache | Private |
| In-memory | inmemory |  | None | Private |

!!!note
    There is currently a proposal to decompose each network into multiple pubsub topics for scalability, the pubsub topics will remain prefixed by the network identifier `/ceramic/<network>/<sep>` see [CIP-120](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/cip-120.md)

## Public networks
---

Ceramic has three public networks that can be used when building applications:

- Mainnet
- Testnet Clay
- Dev Unstable

### **Mainnet**

Mainnet is the main public network used for production deployments on Ceramic. Ceramic's mainnet nodes communicate over the dedicated `/ceramic/mainnet` libp2p pubsub topic and use Ethereum's mainnet blockchain (`EIP155:1`) for generating timestamps used in [time events](../streams/event-log.md) for streams. 

### **Clay Testnet**

Clay Testnet is a public Ceramic network used by the community for application prototyping, development, and testing purposes. Ceramic core devs also use Clay for testing official protocol release candidates. While we aim to maintain a high level of quality on the Clay testnet that mirrors the expectations of Mainnet as closely as possible, ultimately the reliability, performance, and stability guarantees of the Clay network are lower than that of Mainnet. Because of this, **the Clay network should not be used for applications in production**. 

Clay nodes communicate over the dedicated `/ceramic/testnet-clay` libp2p pubsub topic and use Ethereum's Gnosis blockchain for generating timestamps used in [time events](../streams/event-log.md) for streams.

### **Dev Unstable**

Dev Unstable is a public Ceramic network used by Ceramic core protocol developers for testing new protocol features and the most recent commits on the develop branch of `js-ceramic`. It should be considered **unstable and highly experimental**; only use this network if you want to test the most cutting edge features, but expect issues.

Dev Unstable nodes communicate over the dedicated `/ceramic/dev-unstable` libp2p pubsub topic and use Ethereum's Goerli testnet blockchains for generating timestamps used in [time events](../streams/event-log.md) for streams. 

## Private Networks
---

You can prototype applications on Ceramic by running the protocol in a local environment completely disconnected from other public nodes. Here "private" indicates that it is independent of the mainnet network, but does **not** imply any confidentiality guarantees. This is still public data.

### **Local**

Local is a private test network used for the local development of Ceramic applications. Nodes connected to the same local network communicate over a randomly-generated libp2p topic `/ceramic/local-$(randomNumber)` and use a local Ethereum blockchain provided by Truffle's [Ganache](https://trufflesuite.com/ganache/) for generating timestamps used in [time events](../streams/event-log.md) for streams. 

## Examples
---

### TypeScript Definitions

```tsx
enum Networks {
  MAINNET = 'mainnet', // The prod public network
  TESTNET_CLAY = 'testnet-clay', // Should act like mainnet to test apps
  DEV_UNSTABLE = 'dev-unstable', // May diverge from mainnet to test Ceramic
  LOCAL = 'local', // local development and testing
  INMEMORY = 'inmemory', // local development and testing
}
```
