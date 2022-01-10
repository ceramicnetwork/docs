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

A genesis commit is the first [commit](#commit) in a [stream](#streams). Genesis commits may be signed by a public key, or unsigned.

### Signed commit

Signed commits are [commits](#commits) that update the [state](#state) of a [stream](#streams). All signed commits need to be cryptographically signed by a public key.

### Anchor commit

Anchor commits are [commits](#commits) that contain a blockchain timestamp, providing an immutable record of time and ordering to other commits in the [stream](#streams), sometimes known as a _proof-of-publication_. Anchor commits are needed since vanilla [merkle DAGs](https://docs.ipfs.io/concepts/merkle-dag/) have no notion of absolute time needed to build consensus.

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

DID methods are implementations of the [DID](#dids) specification. DID methods must specify a name for the method in the form of a string (see below), a description of where the [DID document](#did-document) is stored (or how it is statically generated), and a [DID resolver](#did-resolver) which can return a DID document given a URI that conforms to that particular DID method. There are over 40 DID methods on the W3C's official DID registry. Ceramic can support any DID method if needed, and currently supports the [3ID DID method](../authentication/3id-did/method.md) and the [Key DID method](../authentication/key-did/method.md). DID URIs look like this:

```
did:<method-name>:<method-specific-identifier>
```

### DID document

DID documents are documents which contain metadata about a given DID. At a minimum they should contain cryptographic key material used for signature verification and encryption/decryption. They may be mutable where their keys and content can be changed/rotated (i.e. [3ID DID method](../authentication/3id-did/method.md)) or statically generated where their contents cannot be manually changed (i.e. [Key DID method](../authentication/key-did/method.md)).

### DID resolver

DID resolvers are software libraries responsible for returning a [DID document](#did-document) given a DID string. Each [DID method](#did-methods) has at least one resolver implementation. For all DID methods supported by Ceramic, the corresponding DID resolver must be included in a Ceramic [node](#nodes). Ceramic uses DID resolvers to verify [stream](#streams) transactions by comparing the signature on the transaction to the [controller](#controllers) of the stream. For the transaction to be successfully processed, a public key corresponding to the transaction signature must be present in the DID Document of the [DID](#dids) listed as a controller.

### DID providers

DID providers are software libraries that expose a json-rpc interface which allows for the creation and usage of a [DID](#dids) that conforms to a particular [DID method](#did-method). Usually a DID provider is constructed using a _seed_ that the user controls. When using Ceramic with [streams](#streams) that require DIDs for [authentication](#authentication), applications either need to integrate a DID provider library, which leaves the responsibility of key management up to the application, or a [DID wallet](#did-wallets), which is a more user-friendly experience.

### DID wallets

DID wallets are software libraries or end-user applications that wrap [DID providers](#did-providers) with additional capabilities. [3ID Connect](../authentication/3id-did/3id-connect.md) is the most popular DID wallet SDK that allows users create, manage, and use a 3ID DID method with their existing blockchain wallets, and without needing to install any additional software.

## Network

### Clients

Clients are software libraries that provide developer interfaces to a Ceramic [node](#nodes). Clients are responsible for [authenticating users](#authentication), providing [StreamType](#streamtypes)-specific interfaces for generating [genesis commits](#genesis-commit) and [signed commits](#signed-commit), and providing generic, streamtype-agnostic interfaces for loading or querying streams. A list of Ceramic clients can be found [here](../build/javascript/installation.md).

### Nodes

Nodes are software libraries that provide core protocol functionality for the Ceramic [network](#networks). Nodes are responsible for processing [stream](#streams) updates (in the form of [signed commits](#signed-commit) from [clients](#clients)), storing [state](#state) for the streams that it cares about, responding to queries, networking with other nodes, replicating streams across the network, and sending valid signed commits to an external [anchor service](#anchor-service) for generating [anchor commits](#anchor-commit).

### Anchor service

A Ceramic Anchor Service (CAS) is a hosted "layer-2" solution for generating [anchor commits](#anchor-commit) for many different [stream](#streams) transactions in a scalable, low cost manner. Ceramic [nodes](#nodes) are responsible for sending anchor requests containing a [StreamID](#streamid) and a [CommitID](#commitid) to a CAS, which then batches these transactions into a merkle tree, and includes the merkle root into a blockchain platform in a single transaction (currently [Ethereum](#ethereum)). After the transaction makes its way onto a blockchain, a Ceramic node creates an anchor commit which includes a reference to the blockchain transaction for every anchored stream. A CAS eliminates the need for each stream transaction to have its own corresponding blockchain transaction, which would be slower and more expensive.

### Networks

Networks are collections of Ceramic [nodes](#nodes) that share specific configurations and communicate over a dedicated [libp2p](#libp2p) topic. Networks are discrete from one another. [Streams](#streams) that exist on one network are not discoverable on or portable to another. Currently, Ceramic has three primary networks: [mainnet](#mainnet), [Clay Testnet](#clay-testnet), and [dev unstable](#dev-unstable).

### Mainnet

Mainnet is the Ceramic [network](#networks) used for production deployments. For more information on mainnet, see the Networks page.

### Clay Testnet

Clay Testnet is a Ceramic [network](#networks) used by the community for application prototyping, development, and testing purposes. Ceramic core devs also use Clay for testing official protocol release candidates. For more information on Clay Testnet, see the Networks page.

### Dev Unstable

Dev Unstable is a Ceramic [network](#networks) used by Ceramic core protocol developers for testing new protocol features and the most recent commits on the `develop` branch of js-ceramic. It should be considered unstable and highly experimental.

## Underlying technologies

### IPFS

[IPFS](https://ipfs.io) is the Interplanetary File System. Simply put, IPFS is a way to address static content using [CIDs](#cid) and to discover this content over a peer-to-peer network of nodes. Ceramic relies on IPFS for storing the [commits](#commits) that make up [streams](#streams) and discovering this data over the [network](#networks).

### CID

A CID (content identifier) is an immutable identifier for a discrete piece of static content stored on [IPFS](#ipfs). CIDs are essentially a hash of the content along with metadata that describes how the content is encoded. Ceramic [streams](#streams) consist of multiple CIDs, encoded using [dag-jose](#dagjose) (and other formats such as dag-cbor), and linked together using [IPLD](#ipld).

### IPLD

[IPLD](https://ipld.io) (Interplanetary Linked Data) is the data structures layer of [IPFS](#ipfs). It is used to link multiple [CIDs](#cid) together into higher-level linked-data structures. Ceramic uses IPLD to create the data structures for [streams](#streams).

### DagJOSE

[DagJOSE](https://specs.ipld.io/block-layer/codecs/dag-jose.html) is a codec for [IPLD](#ipld) which stores content in [IPFS](#ipfs) using IETF's JOSE (JSON object signing and encryption) format. With DagJOSE, each data object actually consists of two separate but linked [CIDs](#cid). It supports both _signed_ and _encrypted_ objects. JWS is used for _signed_ objects and it encodes the payload as a CID, which means that the actual payload is a separate [IPLD](#ipld) object. JWE is used for _encrypted_ objects, and it requires the ciphertext to be a CID in order to not leak the full cleartext. A separate _inline CID_ is used to encode the entire cleartext. For more information refer to the [DagJOSE spec](https://specs.ipld.io/block-layer/codecs/dag-jose.html).

### Libp2p

[Libp2p](https://libp2p.io) is the peer-to-peer networking protocol that is used by Ceramic. It is included as part of the [IPFS](#ipfs) stack. Ceramic relies on libp2p for discovering data over the [network](#networks) and communicating between [nodes](#nodes). Libp2p is also used by other major decentralized platforms such as [Ethereum (Eth2)](#ethereum) and Polkadot.

### Ethereum

[Ethereum](https://ethereum.org) is the world's leading public, permissionless smart contract blockchain platform. Ceramic uses Ethereum for generating the timestamps contained within [anchor commits](#anchor-commit).
