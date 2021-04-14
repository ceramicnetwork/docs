# Tile Document

Tile Document is a StreamType that stores a JSON document, providing similar functionality as a NoSQL document store.

## Use cases

Tile Document streams are commonly used as a NoSQL database replacement for:

- Identity metadata (i.e. profiles, social graphs, reputation scores, linked social accounts)
- User-generated content (i.e. blog posts, social media, etc)
- Indexes of other streams to form collections and user tables (i.e. IDX)
- DID documents (i.e. 3ID DID)
- Verifiable claims
- And more

## Design

- Data structure: Single log
- Content requirements: JSON
- Authetication: DIDs
- Ordering: Anchor records that rely on blockchain checkponting (via blockheight)
- Conflict resolution: Earliest anchor wins
