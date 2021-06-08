# Glossary of terms
This page contains a glossary of terms for Ceramic. Consider this list a work-in-progress; we will continue to update it until it's complete.


## Core concepts

### Streams
Streams are DAG-based data structures for storing continuous, mutable streams of content on [IPFS](#ipfs) and tracking [state](#state) in a completely decentralized, peer-to-peer way. When syncing or loading a stream from the [network](#networks), you will always get back the current state.

### StreamID
A StreamID is an immutable identifier for a [stream](#stream). StreamIDs enable continuous streams of data to be referenced by a persistent identifier instead of by constantly-changing IPFS [CIDs](#cid).

### StreamTypes
StreamTypes are functions used for processing updates to [streams](#streams). StreamTypes handle everything from defining the data structure of the stream, to what can be stored in its [commits](#commits), its state transitition function, [authentication](#authentication) requirements, and [conflict resolution strategy](#conflict-resolution-strategy). Every stream must specify a StreamType; and StreamTypes run on Ceramic [nodes](#nodes). Ceramic comes [pre-installed with various StreamTypes](../streamtypes/overview.md), or you can code your own.

### Commits
Commits are individual [IPFS](#ipfs) records that make up a [stream](#streams). Streams may contain one or more commits.

### Genesis commit
A genesis commit is the first [commit](#commit) in a [stream](#streams).

### Signed commit
Signed commits are [commits](#commits) that update the [state](#state) of a [stream](#streams).

### Anchor commit
Anchor commits are [commits](#commits) that contain a blockchain timestamp, providing an immutable record of time and ordering to other commits in the [stream](#streams), sometimes known as a *proof-of-publication*.

### CommitID
A commitID is an immutable identifier for a specific [commit](#commits) in a [stream](#streams).

### State
State represents the state of a [stream](#streams) at various points in time. When a stream is loaded or queried from the [network](#networks), the current state is returned.

### Tip
A tip is the [CID](#cid) for the most recent [commit(s)](#commits) of a [stream](#streams).

### Conflict resolution strategy
A conflict resolution strategy is logic defined by a [StreamType](#streamtypes) that describes how the protocol should handle conflicting updates to a [stream](#streams) that uses this StreamType.

### Controllers
Controllers are entities allowed to perform updates to a [stream](#streams), by creating new [signed commits](#signed-commit). A given stream may have one or more controllers.


## Stream authentication

### Authentication
Authentication allows a user to perform protected operations on a stream, such as creating [genesis commits](#genesis-commit), [signed commits](#signed-commit), or decrypting data. Each [StreamType](#streamtypes) implementation is able to specify its own authentication mechanism as long as the signatures can be resolved/validated by Ceramic, but most StreamTypes use [DIDs](#dids).

### DIDs
DIDs is the [W3C standard](https://www.w3.org/TR/did-core/) for decentralized identifiers. The DID specification outlines a standard URI scheme for creating a persistent decentralized identifier (DID) for a given subject as well as resolving metadata about that identifier via a [DID document](#did-document). DIDs are used as an [authentication](#authentication) mechanism by most [StreamTypes](#streamtypes).


### DID methods
DID methods are implementations of the [DID](#dids) specification. DID methods must specify a name for the method in the form of a string (see below), a description of where the [DID document](#did-document) is stored (or how it is statically generated), and a [DID resolver](#did-resolver) which can return a DID document given a URI that conforms to that particular DID method. There are over 40 DID methods on the W3C's official DID registry. Ceramic can support any DID method if needed, and currently supports the [3ID DID method](../authentication/dids/3id.md) and the [Key DID method](../authentication/dids/key.md). DID URIs look like this:

```
did:<method-name>:<method-specific-identifier>
```

### DID document
DID documents are documents which contain metadata about a given DID. At a minimum they should contain cryptographic key material used for signature verification and encryption/decryption. They may be mutable where their keys and content can be changed/rotated (i.e. [3ID DID method](../authentication/dids/3id.md)) or statically generated where their contents cannot be manually changed (i.e. [Key DID method](../authentication/dids/key.md)).

### DID resolver
DID resolvers are software libraries responsible for returning a [DID document](#did-document) given a DID string. Each [DID method](#did-methods) has at least one resolver implementation. For all DID methods supported by Ceramic, the corresponding DID resolver must be included in a Ceramic [node](#nodes).

### DID providers
DID providers are software libraries that allow developers or other programs to create, manage, and use [DIDs](#dids) that conform to a particular [DID method](#did-method). When using Ceramic with [streams](#streams) that require DIDs for [authentication](#authentication), applications either need to integrate a DID provider library, which leaves the responsibility of key management up to the application, or a [DID wallet](#did-wallets), which is a more user-friendly experience.

### DID wallets
DID wallets are software libraries or end-user applications that wrap [DID providers](#did-providers) with additional capabilities. [3ID Connect](../authentication/wallets/3id-connect.md) is the most popular DID wallet SDK that allows users create, manage, and use a 3ID DID method with their existing blockchain wallets, and without needing to install any additional software.

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

### DagJOSE
Dag-Jose is a codec for [IPLD](#ipld) which stores content in [IPFS](#ipfs) using IETF's JOSE (JSON object signing and encryption) format. Each dag-jose object is actually two [IPLD](#ipld) objects: one to store the metadata and one to store the payload. This makes it safe for encryption without leaking unnecessary metadata.

### Libp2p
[Libp2p](https://libp2p.io) is the peer-to-peer networking protocol that is used by Ceramic. It is included as part of the [IPFS](#ipfs) stack. Ceramic relies on libp2p for discovering data over the [network](#networks) and communicating between [nodes](#nodes).

### Ethereum
[Ethereum](https://ethereum.org) is the world's most leading public, permissionless smart contract blockchain platform. Ceramic uses Ethereum for generating the timestamps contained within [anchor commits](#anchor-commit).
