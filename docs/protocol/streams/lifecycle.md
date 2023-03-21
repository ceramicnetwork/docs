# **Stream Lifecycle**

## **Write Lifecycle**

---

### **Create**

A stream is created when an [Init Event](event-log.md) is created and published. The stream is then uniquely referenced in the network by its [StreamId](uri-scheme.md), which is derived from this Init Event. 

### **Update**

Updates to a stream include the creating and publishing of data events or timestamp events. When creating these events they must reference the latest event or tip in the stream. The latest event, if there is multiple, is determined by locally following the conflict resolution and [consensus rules](consensus.md). The current update protocol is described further [here](../networking/tip-gossip.md). 

The data event is a signed event and is expected to be created and published by the controller of the given stream it is being appended. A timestamp event on the other hand can be created by any participant in network, given that it is a valid timestamp proof. Typically in the Ceramic network they will be created and published by a timestamping service. 

## **Read Lifecycle**

---

### **Query**

The network can be queried to discover the latest tips for any stream by StreamId. Knowing both the StreamId and tip then allows any node to sync the stream. Query requests are broadcast to the entire network to discover peers that have tips for any given stream. Future query protocols can be optimized and include other stream attributes and values to discover streams and stream tips. The current query protocol is described further [here](../networking/tip-queries.md). 

### **Sync**

Streams can be synced and loaded by knowing both the StreamId and the latest event (tip). Given the latest tip you can traverse the stream event log from event to event in order until the Init Event is reached. Each event is loaded from peers in the network, any peer with a tip is expected to have the entirety of the event stream log. The current sync protocol is described further [here](../networking/event-fetching.md) 

## **Durability**

---

### **Maintenance**
A stream is a set of [events](event-log.md) and these events are stored in IPFS nodes. As long as the entire set of events is pinned and advertised on the IPFS DHT, the respective stream will be retrievable. If your application depends on a stream remaining available, it is your application's responsibility to maintain and store all of its events. This can be done by running your own IPFS nodes or by using an IPFS pinning service. Typically you will be running an IPFS node with Ceramic. 

If any events are not available at a given time, it is not a guarantee that the stream has been deleted. A node with a copy of those events
may be temporarily offline and may return at some future time.

Other nodes in the network can pin (maintain and store) events from your streams or anyone else's streams. If you suffer a data loss, some other node MAY have preserved your data. Popular streams and their events are likely to be stored on many nodes in the network. 
