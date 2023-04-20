# Tip Gossip

When a stream is updated, the latest event (tip) is gossiped and propagated out to all the nodes in a network that are interested in that particular stream. Additionally, listening for all tips, allows a node to learn about streams it did not know about. This allows all interested nodes in the network to quickly get the latest update and state for a stream.

## Protocol
---

### Publishing Updates

When an event is created and appended to a stream, the node will publish an update message to the network. All messages are broadcast on the [libp2p pubsub](https://github.com/libp2p/specs/tree/master/pubsub) topic for the [network](networks.md) this node is configured for. Any other node listening on this network will receive the update and then can decide to take any further action or discard. 

### Update Messages

```tsx
type UpdateMessage = {
  typ: MsgType.UPDATE //0
  stream: StreamID
  tip: CID
  model?: StreamID
}
```

Where:

- **`typ`** - the message is an update message, enum `0`
- **`stream`** - streamId of the stream which this update is for
- **`tip`** - CID of the latest event (tip) of the stream, the update
- **`model`** - streamId of the ComposeDB data model that the stream being updated belongs to (optional)

### Replicating Updates

Any nodes that have received an update message and are interested in that stream can now save the tip (update). Any node that has saved this update can now answer [tip queries](tip-queries.md) for this stream. As long as there is at least one node in the network with this information (tip) saved, the publishing node can go down without effecting the availability of the stream.

## Examples
---

### TypeScript  Definitions

```tsx
/**
 * Ceramic Pub/Sub message type.
 */
enum MsgType {
  UPDATE = 0,
  QUERY = 1,
  RESPONSE = 2,
  KEEPALIVE = 3,
}

type UpdateMessage = {
  typ: MsgType.UPDATE
  stream: StreamID
  tip: CID  // the CID of the latest commit
  model?: StreamID // optional
}

// All nodes will always ignore this message
type KeepaliveMessage = {
  typ: MsgType.KEEPALIVE
  ts: number // current time in milliseconds since epoch
  ver: string // current ceramic version
}
```