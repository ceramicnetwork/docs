# Nodes

Two types of nodes currently work together to support the Ceramic network.

## Ceramic Nodes

---

A Ceramic node is a bundle of services and long-lived processes that support the protocol and provide access to the Ceramic Network. Current implementations bundle and run most all of the following services and sub protocols defined here. This includes the following:

### IPFS Services

The following services are typically provided to a Ceramic node by a connected IPFS node. These services do not necessarily have to be provided through an IPFS node or all bundled together.

| IPLD Blockstore | Stores the underlying IPLD blocks for event streams. Supports block formatting and content addressing.  |
| --- | --- |
| Gossipsub | Several layers of the libp2p stack are used including GossipSub to query streams and broadcast stream updates in the network. |
| Kademlia DHT | A distributed hash table for content and peer lookup and discovery. The same DHT table is used as the primary IPFS network.  |
| Bitswap | Exchange and sync blocks with peers. Allows an event stream to be sycned from one node to another. |

### Ceramic Services

| Pinning | Tracks the streams a node wants to store and to receive the latest events for.  |
| --- | --- |
| StateStore | Tracks and stores the latest tips for pinned streams and caches stream state.  |
| Networking | Runs the stream query and update protocols [LINK] on Gossipsub and manages peer connections.  |
| API | Provides HTTP API service for connected Ceramic clients to read, write and query streams. Additionally, some node management functions are included.  |
| Timestamping | Regularly publishes timestamp proofs and Ceramic time events for a given set of events.  |

!!! note
    In the future, node implementations may only provide a subset of services to the network. For example, nodes may be optimized to provide only indexing, long term storage, client APIs etc.

## Timestamp Nodes

---

Timestamping nodes support a small but important subset of the Ceramic protocol. Timestamping is entirely described by [CAIP-168 IPLD Timestamp Proof](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-168.md) and [Ceramic Time Events](../streams/event-log.md).  Timestamp services aggregate events from streams to be timestamped, construct Merkle proofs, publish transactions and publish timestamp events to the Ceramic Network. Ceramic mainnet currently supports `f(bytes32)`  timestamp transaction types on Ethereum mainnet. This transaction type is entirely described by the [`eip155` namespace](https://github.com/ChainAgnostic/namespaces/blob/main/eip155/caip168.md) for CAIP-168. 

## Implementations

---

The following table includes active node implementations:

| Node | Name | Language | Description | Status | Maintainer |
| --- | --- | --- | --- | --- | --- |
| Ceramic | [js-ceramic](https://github.com/ceramicnetwork/js-ceramic/) | JavaScript | Complete Ceramic implementation. Runs all Ceramic core services, and connects to an IPFS node for all IPFS, libp2p, IPLD services needed.  | Production | 3Box Labs |
| Timestamp | [ceramic-anchor-service](https://github.com/ceramicnetwork/ceramic-anchor-service) | JavaScript | Complete timestamp services. Supports f(bytes32) and raw transaction types for EVM (EIP-155) blockchains.  | Production | 3Box Labs |

Longterm Ceramic is targeting multiple implementations of the protocol to support general resilience, robustness and security. Want to work on a node implementation in a new language like Rust or Go? Get in touch!
