# TileDocument

TileDocument is a StreamType that stores a mutable JSON document, providing similar functionality as a NoSQL document store.

## **Usage**

See [TileDocument API](./api.md) for instructions on how to create and update TileDocument streams. You can query TileDocuments using Ceramic's standard [queries API](../../build/queries.md).

## **Specification**

See [TileDocument StreamType (CIP-8)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md) for the complete specification.

### Storage

TileDocuments are streams used for storing JSON documents. The TileDocument stream is structured as a single log of [commits](../../learn/glossary.md#commits), where each commit only contains the diff from the previous version. Optionally, TileDocuments may specify a JSON schema and all commits must adhere to the schema.

### Consensus

TileDocuments rely on [anchor commits](../../learn/glossary.md#anchor-commit) for providing immutable timestamps for the [genesis commit](../../learn/glossary.md#genesis-commit) and subsequent [signed commits](../../learn/glossary.md#signed-commit) in the stream. In the case of conflicting versions, the branch with the earliest recorded anchor commit will be respected as the canonical branch.

### Authentication

TileDocuments rely on [DIDs](../../learn/glossary.md#dids) for [authentication](../../learn/glossary.md#authentication). Only the DID(s) assigned as the [controller](../../learn/glossary.md#controllers) of the stream are allowed to perform writes. 

## **Sample use cases**

TileDocuments are commonly used for storing:

- Schemas
- Identity metadata and information
- User-generated content (i.e. blog posts, social media, etc)
- Lists of other [streamIDs](../../learn/glossary.md#streamid) to form collections
- [DID Documents](../../learn/glossary.md#did-document) (i.e. 3ID DID Method)
- Verifiable claims
- ...


</br>
</br>
</br>
