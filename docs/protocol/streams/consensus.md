# Consensus

## Consensus model

---

Event streams rely on a limited conflict resolution or consensus model. Global consensus and ordering is not needed for progress and most decisions are localized to the consuming party of a single event stream. Guarantees are limited, but if any two parties consume the same set of events for a stream, they will arrive at the same state. 

The underlying log structure of an event stream allows multiple parallel histories, or branches, to be created resulting in a tree structure. A log or valid event stream is a single tree path from a known “latest” event to the Init Event. Latest events are also referred to as stream `tips`. Logs can have multiple tips when there are branches in the log, and the “tip” selection for the canonical log of a stream becomes a consensus problem. 

### Single stream consensus

A tip and canonical log for a stream are selected by the following pseudo algorithm and rules: 

1. Given a set of tips, traverse each tree path from tip till a commonly shared timestamp event or the Init Event. 
2. From the shared event, traverse each path in the opposite direction (towards tip) until a timestamp event is found. This set of events are considered conflicting events.
3. Given each timestamp event, determine the blockheight for the transaction included in the timestamp proof. Select the path with lowest blockheight. If a single event is selected, exit with path and tip selected, otherwise continue. Most cases will terminate here, it will be rare to have the same blockheight.
4. If multiple tips have the same blockheight, continue traversing path (towards tip) until timestamp event is found on each or tips are reached again.
    1. If only single path includes timestamp proof, exit with path and tip selected
    2. If multiple paths include timestamp events, return to step 3 with new set of events
    3. Otherwise continue
5. If no paths include timestamp proofs, select the path with the greatest number of events from the last timestamp proof till tip. If single path selected, exit with path and tip selected, otherwise continue.
6. If number of events is equal, chooses the event and path which has the smallest CID in binary format (an arbitrary but deterministic choice)

### Cross stream ordering

It’s assumed all timestamp events in a network are committed to the same blockchain, as specified by the `chainId` in the timestamp event. The main Ceramic network commits timestamp proofs to the Ethereum blockchain. 

The addition of timestamp events in streams gives some notion of relative global time for all events time-stamped on the same blockchain. This allows events across different streams to be globally ordered if a higher-level protocol requires it. Ceramic events can also be ordered against transactions and state on the blockchain in which it is timestamped. On most secure blockchains you can also reference wall clock time within some reasonable bounds and order events both in and out of the system based on that. 

## Risks

---

### Late Publishing

Without any global consensus guarantees, all streams and their potential tips are not known by all participants at any point in time. There may be partitions in the networks, existence of local networks, or individual participants may choose to intentionally withhold some events while publishing others. Selective publishing like this may or may not be malicious depending on the context in which the stream is consumed.

Consider the following example: A user creates a stream, makes two conflicting updates and timestamps one of them earlier than the other, but only publishes the data of the update that was timestamped later. Now subsequent updates to the stream will be made on top of the second, published update. Every observer will accept these updates as valid since they have not seen the first update. However if the user later publishes the data of the earlier update, the stream will fork back to this update and all of the other updates made to the stream will be invalidated.

Additionally, note that late publishing may also be used as a deterrent to selling user identities. An identity or account buyer can’t know that the seller is not keeping secret events that they will publish after the identity was sold.