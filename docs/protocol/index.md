# Protocol

## Overview

---

Ceramic is a decentralized event streaming protocol that enables developer to build decentralized databases, distributed compute pipelines, and authenticated data feeds, etc. Ceramic nodes can subscribe to subsets of streams forgoing the need of a global network state. This makes Ceramic an eventually consistent system (as opposed to strongly consistent like L1 blockchains), enabling web scale applications to be built reliably.

The protocol doesn't prescribe how to interpret events found within streams; this is left to the applications consuming the streams. One example of this type of application is ComposeDB.

## Components

---

The Ceramic protocol is made out of various different components:

- [Streams](./streams/index.md)
- [Accounts](./accounts/index.md)
- [Networking](./networking/index.md)
- [API](./api/index.md)
- [Nodes](./nodes/index.md)


## Spec Status

---

### Legend

| Spec state | Label |
| --- | --- |
| Unlikely to change in the foreseeable future. | Stable |
| All content is correct. Important details are covered. | Reliable |
| All content is correct. Details are being worked on. | Draft/WIP |
| Do not follow. Important things have changed. | Incorrect |
| No work has been done yet. | Missing |

### Status by Section

| Section | State |
| --- | --- |
| 1. Streams | Draft/WIP |
| 1.1. Event Log | Reliable |
| 1.2. URI Scheme | Reliable |
| 1.3. Consensus | Draft/WIP |
| 1.4. Lifecycle | Reliable |
| 2. Accounts | Draft/WIP |
| 2.1. Decentralized Identifiers | Draft/WIP |
| 2.2. Permissions | Reliable |
| 2.3. Object-capabilities | Draft/WIP |
| 3. Networking | Draft/WIP |
| 3.1. Tip Gossip | Reliable |
| 3.2. Tip Queries | Reliable |
| 3.3. Event Fetching | Reliable |
| 3.4. Network Identifiers | Reliable |
| 4. API | Missing |
| 5. Nodes | Draft/WIP |

## Next Steps

---

Start learning about [Streams](./streams/index.md), the core data structures used in the Ceramic protocol.