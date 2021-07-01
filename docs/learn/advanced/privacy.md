# Privacy
This page describes various privacy aspects of the Ceramic protocol. Measures we've taken so far, planned future measures, and future research directions.

## Ceramic Protocol
The StreamType system is already quite flexible in terms of how streams can provide developers with privacy features.

### Default properties of Tile and Caip10Links
Currently if you create a TileDocument or Caip10Link stream any data put into it will be public by default. It's however possible to encrypt the content put into the stream by accessing the DID instance from Ceramic (`ceramic.did`) and using it's encryption functionality as described in the [How to store signed and encrypted data on IPFS](https://blog.ceramic.network/how-to-store-signed-and-encrypted-data-on-ipfs/) blog post.

### Confidential streams
A planned improvement to the TileDocument StreamType is to add confidentiality. This basically means that the content of each update to the stream is encrypted by a symmetric key. Whenever a Ceramic node syncs the stream it would only be able to read the stream content if it has the symmetric key for this stream. Stream metadata such as which DID signed the update, in what order, and when it was anchored would still be public.

A StreamType without history like [DIDPublish](https://github.com/ceramicnetwork/CIP/issues/105) could improve the situation somewhat since nodes that sync the stream would not see historical updates.

### Private streams
A fully private stream would mean that all of the data is encrypted.

One approach to this is the one taken by [Textile ThreadsDB](https://textile.io/) which separates the notion of a *follow key* and a *content key*. This allows certain nodes to read the metadata and pin the stream while other nodes can't see anything at all into the stream. So essentially streams are still only confidential to the trusted set of peers that have the *follow key*.

A better approach could be to use some sort of zero-knowledge proof system to keep track of the tip of a set of streams anonymously. This is still a completely open research topic but would likely add some overhead to the party reading the stream. One upside here would be that this system could operate completely trustlessly as opposed to Textile ThreadsDB.
