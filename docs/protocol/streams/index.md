# Streams

Data structures core to Ceramic

### Overview

Streams are a core concept in Ceramic, they include a primary data structure called an event log, a URI scheme to identify unique streams in a network, a simple consensus model to agree on the same event log across the network, and a supporting lifecycle of creating, updating, querying, and syncing streams. 

### [Event Log](event-log.md)

The core data structure of streams is a self-certifying event log. It combines IPLD for hash linked data and cryptographic proofs to create an authenticated and immutable log. This event log can be used to model mutable databases and other data structures on top.

### [URI Scheme](uri-scheme.md)

A URI scheme is used to reference unique streams and unique events included in streams. They use a self describing format that allows anyone to parse and consume a stream correctly, while also easily supporting future changes and new types. 

### [Consensus](consensus.md)

An event log or stream can end up with multiple branches or tips across nodes in the network. Different branches will result in differing stream state. A simple consensus model is used to allow all nodes whom consume the same set of events to eventually agree on a single log or state. 

### [Stream Lifecycle](lifecycle.md)

A stream write lifecycle includes its creation and updates, otherwise know as events. A stream read lifecycle includes queries and syncing. 
