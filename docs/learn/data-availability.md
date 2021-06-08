# Data Availability
This page describes the data availability and persistence model for [streams](./glossary.md#streams) on Ceramic.

## **Overview**
Stream data can be divided into two main categories: [commits](./glossary.md#commits) and [state](./glossary.md#state). While there is some overlap, these two types of data have enough difference that they should be considered separately when planning how to host and persist your streams.

## **Stream commits**
Every stream is an event log consisting of one of more commits, and each commit contains of at least two [IPLD](./glossary.md#ipld) objects. Collectively these commits store the data that makes up the content of a stream.

### Caching
Ceramic nodes have a built-in caching mechanism for short-term storage of commits. Whenever a Ceramic node performs a [write](../build/writes.md) or a [query](../build/queries.md) on a stream, all commits for that stream are first synced from the [network](./glossary.md#networks) and automatically loaded into that node's in-memory cache. This results in the most popular streams being replicated the most, providing some level of data availability. However to preserve disk space and node resources, in-memory cache is currently limited to 50 streams. Once that number is reached, the oldest streams will be evicted from the node's cache in order to make room for newer ones. 

If the node happens to shut down or restart, the cache will be cleared. Without sufficient replication across other nodes due to popularity or additional data persistence measures prior to a shutdown, streams that only exist in-memory will be lost forever. Therefore, cache-only is not a dependable source of data availability for a longer period than a specific session.

### Pinning
Pinning provides a more long-lived mechanism of data persistence for commits. Pinning is a process for instructing a Ceramic node to explicitly host (i.e. "pin") the commits for a specific stream. Since commits are stored in IPLD, Ceramic nodes already contain a bundled [IPFS](./glossary.md#ipfs) node which is where this pinning occurs. IPFS nodes can pin all commits for any stream which is accessible over the Ceramic network to which it is connected. Ceramic pinning can also work using an external IPFS node instead of the bundled internal version.

If developers want the easiest way to make their streams persistent beyond a single session and more resilient against data loss, then pinning is the right option. However, note that if only one IPFS node is pinning a given stream and it disappears forever or gets corrupted, then that stream will be lost. Also, if only one node is pinning a stream (and no other Ceramic nodes have it in cache) and that node goes offline, then that stream will be unavailable to others. Therefore, for improved resilience and data availability it is best to have multiple IPFS nodes running in different envitonments pinning the same streams.

> See the [Pinning](../build/pinning.md) guide for instructions on how to pin streams on a Ceramic node.

### Archiving
Archiving is the most durable, long-lived form of persistence for commits. In addition to caching and pinning, Ceramic developers may also configure their node to connect to an external service for archiving all commits that make up a stream. The exact guarantees provided by archiving differ with each implementation and service provider. For example, archiving to Filecoin provides crypto-economically guaranteed data availability with a pay-as-you-go model, while archiving to Arweave provides crypto-economically guaranteed data availability with a one-time payment model. Conversely, archiving to Amazon S3 provides a simpler model but Amazon cannot guarantee that your data will always be available, but the storage is still more resilient than using pinning and caching only.

## **Stream state**
In addition to the commit log mentioned above which is stored in IPFS, every stream has a [state](./glossary.md#state) which is not stored in IPFS but rather is collectively tracked, persisted, and made available by all Ceramic nodes that are caching and/or pinning the stream.

which is computed by taking the genesis record and applying all commits up to the current [tip]() of the stream. In order to do this, the [StreamType]() logic needs to be applied to every commit in the stream. This provides a mechanism to compute the transformations.

### Caching
Ceramic nodes have built-in caching

### Pinning

### Persistence
