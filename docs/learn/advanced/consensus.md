# Consensus

This page describes the consensus and conflict resolution model for [streams](../glossary.md#streams) on Ceramic.

## **Overview**

Ceramic maintains consensus for individual data [streams](../glossary.md#streams). This is in contrast to traditional blockchain systems where consensus is maintained on the entire global ledger, or to distributed database systems where consensus is maintained for the entire database. Maintaining consensus at the level of individual data objects allows Ceramic to be much more scalable, as nodes only need to track information for the Streams that they care about, rather than for all Streams on the network. This also allows different Streams to use different consensus models - allowing for a flexible and extensible consensus system that can evolve over time and be tailored to specific use cases. In Ceramic, consensus is handled by individual [StreamTypes](../glossary.md#streamtypes), meaning new StreamTypes introduced in the future may also introduce new consensus mechanisms.

!!! warning

    This page contains lots of information about how Ceramic handles reaching consensus on Streams, but the most important part for most developers to understand is the semantics around [simultaneous updates](#simultaneous-updates).

## **Existing consensus model**

Most of the [existing StreamTypes today](../../docs/advanced/standards/stream-programs/index.md) use diffs encoded with json-patch to represent state transitions, and use the Earliest Anchor Wins rule for conflict resolution.

### Json-patch diffs

In the existing [TileDocument](../../docs/advanced/standards/stream-programs/tile-document.md) StreamType, updates to the document's contents are encoded using [json-patch](http://jsonpatch.com/). The resulting diff goes into a [Ceramic Commit](../glossary.md#commits) and can be used to transform the stream's contents from a previous state to a new state. Syncing a TileDocument involves getting the initial state from the [genesis commit](../glossary.md#genesis-commit), then applying the json-patch diffs from each subsequent commit to the content, one at a time, until the end of the stream's commit log, at which point you have the current state of the content.

### Write conflicts

Sometimes, two conflicting logs for the same Stream might exist simultaneously. This can happen when the controller of the stream makes conflicting updates to the same stream on different devices or via different applications. It can also happen if a single Stream has multiple end users who are able to author updates to the stream (either because the stream has multiple controller DIDs, or because the DID method being used as the stream controller allows multiple users/private keys to sign messages on its behalf, like the [did:safe DID method](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-101/CIP-101.md), for example).

Whatever the reason for the diverging logs for a single Stream, it is important that all nodes can come to agreement (consensus) as to which is the correct log for that Stream. Most StreamTypes currently rely on the Earliest Anchor Wins strategy for resolving conflicts between stream logs.

#### Earliest Anchor Wins

Updates to Ceramic Streams are periodically [anchored](../glossary.md#anchor-commit) onto a blockchain (currently Ethereum). This immutable proof-of-publication is used to get a trustless timestamp for when the update occurred. This allows us to safely compare the timestamps associated with different branches of a Stream's log to determine which update happened first. When there are conflicting histories for a Stream log and one branch was anchored earlier than the other, the branch that was anchored earlier wins. If one branch was anchored and the other not, then the branch that was anchored is preferred.

#### Longest update chain

The Earliest Anchor Wins rule can solve many problems related to coming to consensus on a Stream's state, but still has issues if multiple updates are created quickly. Since anchors only happen periodically (depends on the anchor service being used, but currently twice a day for the anchor service that 3Box Labs operates for Ceramic mainnet), multiple updates can be created and published before any of them get anchored. In that case, we still need to come to consensus on the current state. When there are conflicting logs for a stream neither of which have been anchored, we prefer whichever log is longer. This ensures that the most active history with the most updates is preserved. If there are conflicting unanchored branches that have the same length, then the system picks the winning log arbitrarily, but deterministically, to ensure that all nodes come to agreement on the same log, even if there is no good information available to use to decide which to prefer. This can result in writes being lost in certain rare instances where there are conflicting updates published within a few seconds of each other.

#### Simultaneous updates

Ceramic Streams today are updated using a read-modify-write approach. This means that write conflicts can result in writes being lost in some specific scenarios. Consider an app that wants to add a new entry to an array contained within a [TileDocument](../../docs/advanced/standards/stream-programs/tile-document.md) stream. The app loads a stream, gets the current contents of the Stream (including the current value of the array to be updated), adds an element into the array locally, then issues an update to Ceramic with the new contents. The code for this may look something like:

```javascript
const streamId = '<...>' // A StreamID for an existing Ceramic Stream
const doc = await TileDocument.load(ceramic, streamId)
// doc's content contains a 'friends' array field with the value ['mohsin', 'liz']
const content = doc.content
content.friends.push('sergey')
await doc.update(content)
```

If, while this code is running, another update is made to this same Stream (using code that looks much the same but say adding 'stephanie' instead of 'sergey'), then this code may not be aware of that update when the new state is published to the network via the `doc.update` call. You could wind up with two conflicting updates published to the network, one adding 'sergey' to the list, the other adding 'stephanie' to the list. In this case the network will eventually come to consensus about which update to keep, but which one is chosen is arbitrary, and the rejected update will be lost forever. This means that there is a chance that the final array winds up being `['mohsin', 'liz', 'sergey']`, and it is equally possible that the final array winds up being `['mohsin', 'liz', 'stephanie']`, with no way to tell in advance which update will win out.

This behavior is usually not a problem for Ceramic, since most Ceramic streams are controlled by a single end user, who will not be making multiple simultaneous updates to a given stream at the same time. This does mean, however, that Ceramic is not well suited at the moment to applications that depend on allowing multiple end users to update a single stream simultaneously. Note that so long as updates happen more than approximately 30 seconds apart, that should be enough time for the updates to be shared across the entire Ceramic network and prevent conflicts like these from occurring. Also, future StreamTypes may not be subject to this issue, as future StreamTypes will be able to use different consensus mechanisms better suited to handling simultaneous updates.

## **Future improvements**

In the future, Ceramic plans to offer StreamTypes that use [CRDTs](https://en.wikipedia.org/wiki/Conflict-free_replicated_data_type) to encode updates to Streams, which will allow simultaneous updates to be merged automatically without conflict. Such plans, however, are currently still in the research phase.

## **Further reading**

If you want more, lower-level details about how Ceramic maintains consensus on Stream logs, you can read the [specification](https://github.com/ceramicnetwork/ceramic/blob/master/SPECIFICATION.md).
