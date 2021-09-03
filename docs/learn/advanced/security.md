# Security

This page describes various security aspects of the Ceramic protocol and miscellaneous systems like 3ID Connect.

## Ceramic Protocol

The main security properties that makes up Ceramic are cryptographic signatures, *proof-of-publication* (through blockchain anchors), and hash-linked data structures. Together these properties allows the construction of a verifiable data structure. In addition to this Ceramic relies on libp2p pubsub to gossip about stream updates.

### Stream data structure

In general streams are made up of commits which are linked together as a DAG using hash links. This data structure is represented using [IPLD](https://ipld.io/). Commits always contain some sort of proof, usually this is a signature or a *proof-of-publication*. When a Ceramic node syncs a stream this linked DAG of commits can be verified locally and thus be fully trusted.

*Signed commits* are generally signed by the controller of the stream. Different StreamTypes may have different rules about how the signature should be encoded. The TileDocument StreamType uses *dag-jose* to encode signatures. This format is recommended to be used by any new StreamTypes.

A *proof-of-publication* is a proof that some content was published at some point in time. This is achieved by publishing the hash of the content on a blockchain. These proofs can be made more cost effective by putting multiple content hashes as leaves in a merkle tree and only publishing the root. The *proof-of-publication* provides a definite ordering of events in a stream, which is useful when you want to do key revocation in a secure manner.

The *linked commit DAG* provides a logical ordering of events which is useful when constructing streams that use CRDT type logic. For a detailed description of these benefits reference the [Merkle-CRDTs](https://research.protocol.ai/blog/2019/a-new-lab-for-resilient-networks-research/PL-TechRep-merkleCRDT-v0.1-Dec30.pdf) paper.

#### Conflict resolution strategies

Different StreamTypes in Ceramic may have different conflict resolution strategies with different security properties.

The *earliest anchor rule* solves any conflict in the history of a stream by picking the stream that was anchored at the earliest point in time. This enables keys to be securely revoked since someone that gains possession of an old key after it was revoked will be unable to produce a *proof-of-publication* that is earlier than the first anchor. This is used in the [3ID DID Method](../../authentication/3id-did/method.md) which registers a set of public keys associated to a DID URL, when the 3ID stream is updated with new public keys, revoking the old keys, an outside observer can be sure of the state of the 3ID without any secondary sources of information. 3IDs are built on top of the [TileDocument](../../streamtypes/tile-document/overview.md) stream type, and alternative DID methods could also be built on top of it.

A drawback with this strategy is that the controller of the stream can create anchored commits in secret and reveal them at a later point in time to change the history of the stream. There are various ways to mitigate this. One of them is to only allow anchor services that actively make sure that the update is valid before it's being anchored.

The *latest nonce rule* solves any conflict by simply picking the signed commit which includes the largest nonce. This strategy doesn't support key revocation, so the controller of a stream with this strategy can't be changed. Ceramic doesn't support this strategy yet, but it is required for StreamTypes like [DIDPublish](https://github.com/ceramicnetwork/CIP/issues/105). In the future The Caip10Link StreamType may also be updated to use this strategy for efficiency reasons.

### Network gossip
Ceramic nodes use libp2p pubsub to gossip about stream tips. The two operations that happen in the gossip is publication of stream updates and queries for tips by nodes that load a specific stream. The main security consideration for a Ceramic node is that any new tip that comes in for a stream which it cares about could be a fake or invalid tip for that stream and thus needs to be verified.

#### DoS attack
A malicious node can spam the pubsub topic by sending a lot of messages. This may be randomly generated messages that are not even for valid streams, or it could be invalid tips for valid streams that get rejected by the stream update rules (the "false log attack" explained below). The main counter attack for this is to limit the amount of messages a single node can send by having an automated reputation system that disconnects from nodes which are spammy. No such reputation systems exists in ceramic today, though it is planned as a future improvement.

#### False log attack
A malicious node can spam nodes that pin specific streams by sending faulty commit logs. The unsuspecting node would sync the log of the stream and find it invalid, at which point the node would throw out the faulty log without applying it to its local copy of the stream's state. However, if the false log is long and the malicious node sends multiple of these it may cause a significant amount of overhead. The most simple countermeasure to this is to simply stop accepting tips from nodes which have proven to not be reliable. A more significant approach would be to build a StreamType that includes a recursive zero-knowledge proof that the log is indeed correctly associated with the given streamid.

#### Caip10Link clock synchronization
Currently the Caip10Link streamtype relies on system time on the client machine to mitigate replay attacks. A new update to a Caip10Link stream is only valid if the proof includes a timestamp that is larger than the timestamp of the previous update. This presents a problem however if the users system time for some reason is incorrectly far into the future. Users would at this point be unable to prevent a replay attack using a proof created on this machine. The replay attack could be used to reset a Caip10Link to point to any DID it had previously been linked to. A possible workaround could be to switch to using a `nonce` which is always incremented by one, or include the `prev` pointer to the previous commit in the proof.
