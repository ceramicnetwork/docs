# Event Fetching

Once a tip is discovered through the [Tip Gossip](tip-queries.md) or [Tip Query](tip-queries.md) protocols a node knows both the StreamID and the latest event CID of the stream. The latest Event contains the CID of the `prev` Event and so on until the Init Event is found in the event log. The Init Event's CID is also in the StreamID. This is proof that the tip is part of the stream identified by the StreamId.

The tip is one of [Init, Data, or Time Event](../streams/event-log.md). If the tip CID is the initial event CID then the stream has never been updated and the initial event is the complete event log. If the tip CID points to a Data, or Time event then that event will contain a `prev` field with a CID link to its previous event. IPFS can be used to retrieve this event. Similarly you can use IPFS to recursively fetch and resolve every `prev` event in an event log until reaching the initial event. At that point you have retrieved and synced the entire stream. 

Fetching an event with IPFS from a peer both relies on [IPFS BitSwap](https://docs.ipfs.tech/concepts/bitswap/) and the [IPFS DHT](https://docs.ipfs.tech/concepts/dht/).