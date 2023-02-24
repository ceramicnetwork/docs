# **Ceramic Protocol**

Ceramic is a decentralized event streaming protocol that enables developers to build decentralized databases, distributed compute pipelines, and authenticated data feeds, etc. Ceramic nodes can subscribe to subsets of streams forgoing the need of a global network state. This makes Ceramic an eventually consistent system (as opposed to strongly consistent like L1 blockchains), enabling web scale applications to be built reliably.

The protocol doesn't prescribe how to interpret events found within streams; this is left to the applications consuming the streams. One example of this type of application is ComposeDB.

## **Core Components**

---

The Ceramic protocol consists of the following components:

- [**Streams →**](./streams/index.md)
- [**Accounts →**](./accounts/index.md)
- [**Networking →**](./networking/index.md)
- [**Ceramic API →**](./api/index.md)
- [**Ceramic Nodes →**](./nodes/index.md)


## **Specification Status**

---

| Section | State |
| --- | --- |
| [1. Streams](./streams/index.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./streams/index.md)** |
| [1.1. Event Log](./streams/event-log.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./streams/event-log.md)** |
| [1.2. URI Scheme](./streams/uri-scheme.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./streams/uri-scheme.md)** |
| [1.3. Consensus](./streams/consensus.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./streams/consensus.md)** |
| [1.4. Lifecycle](./streams/lifecycle.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./streams/lifecycle.md)** |
| [2. Accounts](./accounts/index.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./accounts/index.md)** |
| [2.1. Decentralized Identifiers](./accounts/decentralized-identifiers.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./accounts/decentralized-identifiers.md)** |
| [2.2. Authorizations](./accounts/authorizations.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./accounts/authorizations.md)** |
| [2.3. Object-Capabilities](./accounts/object-capabilities.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./accounts/object-capabilities.md)** |
| [3. Networking](./networking/index.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./networking/index.md)** |
| [3.1. Tip Gossip](./networking/tip-gossip.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./networking/tip-gossip.md)** |
| [3.2. Tip Queries](./networking/tip-queries.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./networking/tip-queries.md)** |
| [3.3. Event Fetching](./networking/event-fetching.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./networking/event-fetching.md)** |
| [3.4. Network Identifiers](./networking/networks.md) | **[<span style="color:rgba(68, 131, 97, 1)">Reliable</span>](./networking/networks.md)** |
| [4. API](./api/index.md) | **[<span style="color:rgba(212, 76, 71, 1)">Missing</span>](./api/index.md)** |
| [5. Nodes](./nodes/index.md) | **[<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>](./nodes/index.md)** |

#### **Legend**

| Spec state | Label |
| --- | --- |
| Unlikely to change in the foreseeable future. |  **<span style="color:rgba(51, 126, 169, 1)">Stable</span>** |
| All content is correct. Important details are covered. | **<span style="color:rgba(68, 131, 97, 1)">Reliable</span>** |
| All content is correct. Details are being worked on. | **<span style="color:rgba(203, 145, 47, 1)">Draft/WIP</span>** |
| Do not follow. Important things have changed. | **<span style="color:rgba(217, 115, 13, 1)">Incorrect</span>** |
| No work has been done yet. | **<span style="color:rgba(212, 76, 71, 1)">Missing</span>** |


## **Next Steps**

---

Start learning about [Streams](./streams/index.md), the core data structures used in the Ceramic protocol.
