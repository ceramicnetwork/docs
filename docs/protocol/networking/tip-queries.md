# Tip Queries

Ceramic streams are identified by a [URI](../streams/uri-scheme) called StreamIds. Nodes that want to sync a stream need to query the network for the tip of that stream using its StreamId. 

!!!note
    Tips are the most recent Init, Data, or Time event for a given Stream Tip


## Protocol
---

A node resolving a Ceramic URI sends a query message to the network and then listens for responses with the candidates for the current tip of the stream. Any node that is interested in the same stream on the network and has stored its tips will respond with a response message. All messages are sent on the [libp2p pubsub](https://github.com/libp2p/specs/tree/master/pubsub) topic for the [network](networks.md) the node is configured for.

### **Query Message**

```tsx
type QueryMessage = {
  typ: MsgType.QUERY // 1
  id: string
  stream: StreamID
}
```

Where:

- **`typ`** - the message is a query message, enum `1`
- **`stream`** - the streamId that is being queried or resolved
- **`id`** - a multihash `base64url.encode(sha265(dagCBOR({typ:1, stream: streamId})))`, can generally be treated as a random string that is used to pair queries to responses

### **Response Message**

```tsx
type ResponseMessage = {
  typ: MsgType.RESPONSE // 2
  id: string
  tips: Map<StreamId, CID> 
}
```

Where:

- **`typ`** - the message is a response message, enum `2`
- **`id`** - id of the query that this message is a response to
- **`tips`** - map of `StreamID` to CID of stream tip

!!!note
    Currently this will only ever have a single `StreamID` in the query, but Ceramic will likely have batch queries at some point in the future.


## Examples
---

### TypeScript Definitions

```tsx
enum MsgType { // Ceramic Pub/Sub message type.
  UPDATE = 0,
  QUERY = 1,
  RESPONSE = 2,
  KEEPALIVE = 3,
}

type QueryMessage = {
  typ: MsgType.QUERY
  id: string
  stream: StreamID
}

type ResponseMessage = {
  typ: MsgType.RESPONSE
  id: string
  tips: Map<string, CID>
}
```
