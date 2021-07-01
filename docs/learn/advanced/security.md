# Security

This page describes various security aspects of the Ceramic protocol and miscellaneous systems like 3ID Connect.

## Ceramic Protocol

The main security properties that makes up Ceramic are cryptographic signatures, *proof-of-publication* (though blockchain anchors), and hash-linked data structures. Together these properties allows the construction of a verifiable data structure. In addition to this Ceramic relies on libp2p pubsub to gossip about stream updates.

### Stream data structure

In general streams are made up of commits which are linked together as a DAG using hash links. This data structure is represented using [IPLD](https://ipld.io/). Commits always contain some sort of proof, usually this is a signature or a *proof-of-publication*. When a Ceramic node syncs a stream this linked DAG of commits can be verified locally and thus be fully trusted.

*Signed commits* are generally signed by the controller of the stream. Different StreamTypes may have different rules about how the signature should be encoded. The TileDocument StreamType uses *dag-jose* to encode signatures. This format is recommended to be used by any new StreamTypes.

A *proof-of-publication* is a proof that some content was published at some point in time. This is achieved by publishing the hash of the content on a blockchain. These proofs can be made more cost effective by putting multiple content hashes as leaves in a merkle tree and only publishing the root. The *proof-of-publication* provides a definite ordering of events in a stream, which is useful when you want to do key revocation in a secure manner.

The *linked commit DAG* provides a logical ordering of events which is useful when constructing streams that use CRDT type logic. For a detailed description of these benefits reference the [Merkle-CRDTs](https://research.protocol.ai/blog/2019/a-new-lab-for-resilient-networks-research/PL-TechRep-merkleCRDT-v0.1-Dec30.pdf) paper.

#### Conflict resolution strategies

Different StreamTypes in Ceramic may have different conflict resolution strategies with different security properties.

The *earliest anchor rule* solves any conflict in the history of a stream by picking the stream that was anchored at the earliest point in time. This enables keys to be securely revoked since someone that gains possession of an old key after it was revoked will be unable to produce a *proof-of-publication* that is earlier than the first anchor. A drawback with this strategy is that the controller of the stream can create anchored commits in secret and reveal them at a later point in time to change the history of the stream. However, this can be mitigated by allowing the log to have multiple branches of history and merge them using a CRDT. This strategy is used by the [TileDocument](../../streamtypes/tile-document/overview.md).

The *latest nonce rule* solves any conflict by simply picking the signed commit which includes the largest nonce. This strategy doesn't support key revocation, so the controller of a stream with this strategy can't be changed. Ceramic doesn't support this strategy yet, but it is required for StreamTypes like [DIDPublish](https://github.com/ceramicnetwork/CIP/issues/105). In the future The Caip10Link StreamType may also be updated to use this strategy for efficiency reasons.

### Network gossip
Ceramic nodes use libp2p pubsub to gossip about stream tips. The two operations that happen in the gossip is publication of stream updates and queries for tips by nodes that load a specific stream. The main security consideration for a Ceramic node is that any new tip that comes in for a stream which it cares about needs to be verified.

#### DoS attack
A malicious node can spam the pubsub topic by sending a lot of messages. This may be randomly generated messages that are not even for valid streams. The main counter attack for this is to limit the amount of messages a single node can send by having an automated reputation system that disconnects from nodes which are spammy.

#### False log attack
A malicious node can spam nodes that pin specific streams by sending faulty commit logs. The unsuspecting node would sync the log of the stream and when it validates it the stream would get invalidated and removed from the local node. However, if the false log is long and the malicious node send multiple of these it may cause a significant amount of overhead. The most simple countermeasure to this is to simply stop accepting tips from nodes which have proven to not be reliable. A more significant approach would be to build a StreamType that includes a recursive zero-knowledge proof that the log is indeed correctly associated with the given streamid.


## 3ID Connect

3ID Connect is a key management system for the 3ID DID method. It allows blockchain accounts to be used as an authentication method to the keys used in the users 3ID. In order to authenticate a user signs a message with their wallet. The entropy is used to generate a [Key DID](../../authentication/key-did/method.md) which in turn is used to decrypt the seed used for the 3ID.

The main security concern here is that the seed used for the 3ID is stored within the 3ID iframe. While this restricts the app using 3ID Connect to do whatever it want's with the seed, a malicious browser plugin could easily pick up the seed from the web browser and take full control over the users 3ID. While are multiple paths that can be used to mitigate this issue, it's currently considered to be a fair tradeoff to improve UX for users. In contrast to a users crypto wallet, the 3ID doesn't actually hold or control any cryptocurrency; it only controls data read/writes. This means that the incentive for a hacker to execute an attack is much smaller since there is no immediate financial reward. Also note that this security issue is not inherent to 3ID itself, it's just related to how 3ID is used within 3ID connect.

### Possible security improvements

#### External 3ID controller
The 3ID did document has different public keys used for writing and controlling the DID document itself. One possible mitigation strategy is to store the controller key in a more secure manner. This would make it possible to recover if the day to day writing public keys are compromised. An example solution here could be to store the controller key in a MetaMask Snap.

#### Don't store keys in web context
The long term solution is of course to not store any keys of the 3ID in the web browser context. This could be achieved though wallets supporting 3ID directly, or an Object-based capability system rooted in blockchain accounts which would allow applications to request specific permissions from wallets.
