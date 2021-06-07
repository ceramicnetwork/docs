# Glossary of terms
This page contains a glossary of terms for Ceramic. Consider this list a work-in-progress; we will continue to update it until it's complete.


## Core concepts

### Streams
Streams are DAG-based data structures for storing stateful, mutable content on [IPFS](). Streams provide dynamic data objects that allow you to store, edit, add, and update information.

### StreamID
A StreamID is an immutable identifier for a [stream](). StreamIDs enable streams of data to be referenced by a persistent identifier instead of by constantly-changing IPFS hashes (CIDs). When syncing or loading a StreamID from the Ceramic network, you will always get back the most current version.

### StreamTypes
StreamTypes are functions that are used to process updates to [streams](). StreamTypes handle everything from defining the data structure of the stream, to what can be stored in its commits, its state transitition function, [authentication]() requirements, and [conflict resolution mechanism](). Every stream must specify a StreamType. StreamTypes run on Ceramic nodes. Ceramic comes [pre-installed with various StreamTypes](), or you can code your own.

### Commits
Commits are individual IPFS records that make up a stream. Streams may contain one or more commits.

### Genesis commit
A genesis commit is the first commit in a stream.

### Signed commit
Signed commits are commits that update the state of a stream.

### Anchor commit
Anchor commits are commits that contain a blockchain timestamp, providing an immutable record of time and ordering to commits in the stream.

### CommitID
A commitID is an immutable identifier for a specific commit in a stream.

### State
State represents the state of a stream at various points in time.

### Tip
A tip is the most recent commit of a stream.

### Conflict resolution mechanism
A conflict resolution mechanism is logic defined by a [StreamType]() that describes how the protocol should handle conflicting updates to a [stream]().

### Controllers
Controllers are entities allowed to perform updates to a [stream](), by creating new [signed commits](). A given stream may have one or more controllers.


## Authentication system

### Authentication
Authentication allows a user to perform protected operations on a stream, such as making updates.

### DIDs
DIDs is the W3C standard for decentralized identifiers. The DID specification outlines a standard for creating persistent decentralized identifiers (DIDs) as well as storing and resolving metadata about that identifier (DID document). DIDs are used as an authentication mechanism by various StreamTypes, such as [TileDocument]() and [CAIP10Link]().

### DID methods
DID methods are implementations of the DID specification. There are over 40 DID methods on the W3C's official DID registry. Ceramic can support any DID method, and currently supports the [3ID DID method](), as well as the [Key DID method](). More DID methods can be added to the protocol if needed.

### DID resolvers
DID resolvers

### DID wallets
DID wallets are software that makes it easy to integrate DIDs into your application. [3ID Connect]() is the most popular DID wallet that is compatible with Ceramic, as it supports the 3ID DID method.


## Network

### Clients
Clients are software libraries that provide developer interfaces to a Ceramic [node](#nodes). Clients are responsible for [authenticating users](#authentication), providing [StreamType](#streamtypes)-specific interfaces for generating [genesis commits](#genesis-commit) and [signed commits](#signed-commit), and providing generic, streamtype-agnostic interfaces for loading or querying streams. A list of Ceramic clients can be found [here](./clients.md).

### Nodes
Nodes are software libraries that provide core protocol functionality for the Ceramic [network](#networks). Nodes are responsible for processing [stream](#streams) updates (in the form of [signed commits](#signed-commit) from [clients](#clients)), storing [state](#state) for the streams that it cares about, responding to queries, networking with other nodes, replicating streams across the network, and sending valid signed commits to an external [anchor service](#anchor-service) for generating [anchor commits](#anchor-commit).

### Anchor service
Anchor services are hosted "layer-2" services for Ceramic that generate [anchor commits](#anchor-commit) for many different [streams](#streams) in a scalable, low cost manner by batching many different stream transactions into a merkle tree, and including the merkle root into a transaction on a blockchain platform (currently [Ethereum](#ethereum)). This eliminates the need for each stream transaction to have its own corresponding blockchain transaction, which would be slower and more expensive.

### Networks
Networks are collections of Ceramic [nodes](#nodes) that share specific configurations and communicate over a dedicated [libp2p](#libp2p) topic. Networks are discrete from one another. [Streams](#streams) that exist on one network are not discoverable on or portable to another. Currently, Ceramic has three primary networks: [mainnet](#mainnet), [Clay Testnet](#clay-testnet), and [dev unstable](#dev-unstable).

### Mainnet
Mainnet is the Ceramic [network](#networks) used for production deployments. For more information on mainnet, see the Networks page.

### Clay Testnet
Clay Testnet is a Ceramic [network](#networks) used for community testing and development purposes. For more information on Clay Testnet, see the Networks page.

### Dev Unstable
Dev Unstable is a Ceramic [network](#networks) used by Ceramic core protocol developers for testing new protocol features and release candidates. It should be considered unstable and highly experimental.


## Underlying technologies

### IPFS
[IPFS](https://ipfs.io) is the Interplanetary File System. Simply put, IPFS is a way to address static content using [CIDs](#cid) and to discover this content over a peer-to-peer network of nodes. Ceramic relies on IPFS for storing the [commits](#commits) that make up [streams](#streams) and discovering this data over the [network](#networks).

### CID
A CID (content identifier) is an immutable identifier for a discrete piece of static content stored on [IPFS](#ipfs). CIDs are essentially a hash of the content along with metadata that describes how the content is encoded. Ceramic [streams](#streams) consist of multiple CIDs, encoded using [dag-jose](#dag-jose), and linked together using [IPLD](#ipld).

### IPLD
[IPLD](https://ipld.io) (Interplanetary Linked Data) is the data structures layer of [IPFS](#ipfs). It is used to link multiple [CIDs](#cid) together into higher-level linked-data structures. Ceramic uses IPLD to create the data structures for [streams](#streams).

### Dag-Jose
Dag-Jose is a codec for [IPLD](#ipld) which stores content in [IPFS](#ipfs) using IETF's JOSE (JSON object signing and encryption) format. Each dag-jose object is actually two [IPLD](#ipld) objects: one to store the metadata and one to store the payload. This makes it safe for encryption without leaking unnecessary metadata.

### Libp2p
[Libp2p](https://libp2p.io) is the peer-to-peer networking protocol that is used by Ceramic. It is included as part of the [IPFS](#ipfs) stack. Ceramic relies on libp2p for discovering data over the [network](#networks) and communicating between [nodes](#nodes).

### Ethereum
[Ethereum](https://ethereum.org) is the world's most leading public, permissionless smart contract blockchain platform. Ceramic uses Ethereum for generating the timestamps contained within [anchor commits](#anchor-commit).
