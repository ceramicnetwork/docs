# Features
Ceramic is a decentralized, open source platform for creating, hosting, and sharing streams of data. This page describes the most important features of the protocol.

### **Mutable streams**
Store, edit, or update continuous [streams](./glossary.md#ipfs) of content in stateful data structures on [IPFS](./glossary.md#ipfs). The data structure of streams can be fully customized to your use case and needs.

### **Immutable naming**
Reference streams by a persistent identifier, called a [StreamID](./glossary.md#streamid), instead of needing to keep track of ever-changing IPFS hashes.

### **Custom functions**
Write custom functions for processing updates to your stream's state. These functions are called [StreamTypes](./glossary.md#streamtypes), and they're deployed to a Ceramic [node](./glossary.md#streamid). StreamTypes ingest new updates and autonomously apply transformations to your stream – guaranteeing data consistency and integrity without needing to rely on an external source of compute logic or state management. Ceramic nodes come [prepackaged with common StreamTypes](../streamtypes/overview.md) making it easy to get started without needing to code your own.

### **DID authentication**
Authenticate to and transact with streams using W3C-standard decentralized identities ([DIDs](./glossary.md#dids)). While every StreamType is able to define its own authentication requirements and mechanisms, DIDs are the most common. Notably DIDs can be controlled with one or more Web3/blockchain wallets, so your users can build up a unified cross-chain Web3 identity using the wallets they already have.

### **Scalable consensus**
Unlike blockchains or other DLT systems which have scalability limitations due to a reliance on global state and a single execution environment, consensus ensures data consistency and network scalability

### **Host & deploy**
Make your streams available over a global peer-to-peer network

### **Sync & share**
Query, sync, or subscribe to any stream using standard APIs

### **Archive anywhere**
Backup the contents of your stream to IPFS, Filecoin, or Amazon S3
