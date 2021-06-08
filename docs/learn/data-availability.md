# Data Availability
This page describes the data availability and persistence model for Ceramic data.

## Overview
Ceramic data can be subdivided into two categories: stream commits, and stream state/tips.

## Stream data
Streams consist of one of more commits, each of which are comprised of at least two IPFS objects. Stream data is the data that makes up the commits of a stream.

### Caching
Ceramic nodes have a built-in caching mechanism for short-term storage of stream data. Whenever a Ceramic node performs a [write]() or [read]() on a stream, the state of that stream is added to the node's in-memory cache. Note that in-memory cache is limited to 50/500 streams. Once that number is reached, the oldest streams will be evicted to make room for newer ones in the cache. LevelDB? or what's the short term node. If noes are restarted, then this cache disappears. It is not a dependable source of data availability longer-term than a specific session.

## Pinning
Pinning provides medium-term storage for stream data. Ceramic nodes depend on IPFS nodes; by default, each Ceramic node has a built-in IPFS node, but Ceramic can alternatively be run with an external IPFS node. If you want your stream data to persist beyond any single session, these streams must be pinned by your Ceramic node. See the [Pinning]() guide for instructions on how to pin streams on your Ceramic node.

### Persistence
Ceramic nodes need to connect to external services for long-term storage of stream data. These services can be external IPFS nodes, Filecoin nodes, or Amazon S3.

## Stream State
Stream [state]() is the current state of a stream, which is calculated by taking the genesis record and applying all commits up to the current [tip]() of the stream. In order to do this, the [StreamType]() logic needs to be applied to every commit in the stream. This provides a mechanism to compute the transformations.

### Caching
Ceramic nodes have built-in caching

### Pinning

### Persistence
