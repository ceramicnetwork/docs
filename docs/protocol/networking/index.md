# Networking

Networking sub-protocols for Ceramic

## Overview

---

Ceramic streams and nodes are grouped into independent networks. These networks can be either for public use or for use by a specific community. There are currently a few commonly shared and default networks. When a stream is published in a network, other nodes in the same network are able to query and discover the stream, receive the latest stream events (tips), and sync the entire event set for a stream. Each of the these network functions are defined by a sub protocol listed below.

## Networks

---

Networks are collections of Ceramic [nodes](../nodes.md) that share specific configurations and communicate over dedicated [libp2p](https://libp2p.io/) pubsub topics. They are easily identified by a path string, for example `/ceramic/mainnet` .

[Networks](networks.md)

## Tip Gossip

---

When a stream is updated, the latest event (tip) is gossiped and propagated out to all the nodes in a network that are interested in that particular stream. Listening for all tips, allows a node to learn about streams it did not know about. 

[Tip Gossip](tip-gossip.md)

## Tip Queries

---

Nodes in a network with a specific StreamId can query for the most recent event (tip) of that given stream. Queries enable a node that know about a stream to find the latest event (tip). 

[Tip Queries](tip-queries.md)

## Event Fetching

---

Nodes that have the tip (latest event) of a stream, can use the tip to fetch all prior events in that stream.  Fetching enables a node that knows a tip to sync the entire event set for a stream and learn its latest state. 

[Event Fetching](event-fetching.md)
