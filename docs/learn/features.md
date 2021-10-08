# Features

Ceramic is a decentralized, open source platform for creating, hosting, and sharing streams of data. With Ceramic's permissionless data streaming network, you can store streams of information and ever-changing files directly on the decentralized web – and share updates with anyone in the world. This page describes the most important features of the protocol.

### **Mutable streams**

Store, edit, and update continuous [streams](./glossary.md#streams) of content or ever-changing files using stateful data structures on [IPFS](./glossary.md#ipfs) (specifically [IPLD](./glossary.md#ipld)). The data structure of your streams can be fully customized to your specific use cases and needs.

### **Immutable naming**

Reference streams by a persistent identifier, called a [StreamID](./glossary.md#streamid), instead of needing to keep track of IPFS [CIDs](./glossary.md#cid) that change every time your content does.

### **Tamper-proof history**

Inspect, audit, or time travel through the complete history of any stream to see how its content has changed over time.

### **Global availability**

Host and make your streams available over a decentralized, global, peer-to-peer network that is censorship-resistant and completely free of middlemen.

### **Sync & share**

Query, sync, or subscribe to any stream on the network using standardized APIs, making it easy to share data across organizational boundaries. Whenever you or anyone else syncs your streams from the network, they will always get back the most current state.

### **Limitless composability**

All streams on Ceramic exist within a single global namespace, allowing you to reference and aggregate multiple streams into higher-order compositions or fork and remix existing streams into entirely new creations.

### **Custom functions**

Write custom functions for processing updates to your stream's [state](./glossary.md#state). These functions are called [StreamTypes](./glossary.md#streamtypes), and they're deployed to a Ceramic [node](./glossary.md#nodes). StreamTypes ingest new updates and autonomously apply transformations to your stream – guaranteeing data consistency and integrity without needing to rely on an external source of compute logic or state management. Ceramic nodes come [prepackaged with common StreamTypes](../streamtypes/overview.md) making it easy to get started creating applications without needing to code your own.

### **DID authentication**

[Authenticate](./glossary.md#authentication) to and transact with streams using W3C-standard decentralized identities ([DIDs](./glossary.md#dids)). While every StreamType is able to define its own authentication requirements and mechanisms, DIDs are the most common. Notably DIDs can be controlled with one or more Web3/blockchain wallets, so your users can build up a unified cross-chain Web3 identity using the wallets they already have.

### **Scalable consensus**

Unlike blockchains or other DLT systems which have scalability limitations due to reliances on a single execution environment and global state, Ceramic takea a different approach to network scalability. On Ceramic, every stream maintains its own state and nodes independently process stream transactions, allowing for unbounded parallelization. This enables Ceramic to operate at worldwide data scale, which is orders of magnitude greater than the scale needed for decentralized finance.

### **Archive anywhere**

Backup the contents of your stream to IPFS, Filecoin, or Amazon S3. Visit [Data Availability](./advanced/data-availability.md) to learn more about the data persistence and availability model of Ceramic.
