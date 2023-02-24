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
