# Network Overview
Ceramic is a decentralized network of nodes that run the Ceramic protocol.

## Clients
Clients are software that provide API access to Ceramic nodes. They allow interactions with the Ceramic network.

### Responsibilities

#### API Interface

#### Authenticate users

#### Sign records

#### Add more

### Client Implementations
- [HTTP Client]()
- [JS Client]()
- [CLI Client]()

## Nodes
Nodes are software that run the Ceramic protocol and form a peer-to-peer network with other nodes.

### Networking

#### Network connection
Nodes must specify which Ceramic network they are connecting to. This configuration allows the node to mesh with all other nodes connected to the same network.

#### Gossip updates
Nodes must specify which Ceramic network they are connecting to. This configuration allows the node to mesh with all other nodes connected to the same network.

#### Query responses
Nodes are responsible for responding to queries about any document that it has. If the node has the document in its cache it will respond directly, but if if doesn't have it, it will ask other nodes on the network for it using libp2p.

### Storage

#### Loading documents
Nodes can ask other nodes for a document and it will sync it from the network and load it in memory. This includes the entire document log (contents) and its most recent tip (state).

#### Caching/Pinning documents
Ceramic nodes use an instance of IPFS for short-term pinning/caching the documents that they care about. For each document that it cares about, a node will cache its document log and its tip. Nodes cache the Ceramic nodes come prepackaged with an internal IPFS node, but an externally run IPFS node may be used instead. (add something in here about garbage collection?)

#### Persistence coordination
Ceramic nodes may optionally specify one or more external service(s) for the long-term storage of documents. If specified, the node is responsible for forwarding document records to this service. Learn more about persistence options.

### Transactions

#### Authentication (??)
Nodes take a DID provider instance and allow that authenticated user to perform transactions.

#### Record validation
Nodes receive records (from clients (or anchor services?)) and then validate that these records conform to the rules of the document's specified doctype.

#### Record application
Nodes apply only valid records to the document's document log. Invalid or malformed records are discarded.

#### Anchor service coordination
After applying a genesis record or signed record to the document log, nodes then send these records to the HTTP endpoint of an anchor service which anchors it in a blockchain. After successfully anchoring the record, the anchor service sends back an anchor record over libp2p which is then applied to the document log by the node.

#### Conflict resolution
Should this be its own category, or should this live elsewhere?

## Anchor Services

### Responsibilities

#### Blockchain anchoring
The primary responsibility of an anchor service is to generate anchor records by committing signed records into a blockchain. All of the responsibilities below are in service of this primary responsibility.

#### Merkle tree construction
Constructs a merkle tree of all signed records that will be simultaneously committed to a blockchain in a single hash, called a merkle root.

#### Anchor metadata
Bloom filter, helps with indexing services.

#### Anchor status messages
Sends messages to the pubsub room specified for the Ceramic network that the anchor service is servicing. This allows Ceramic nodesLearn more about anchor status messages.
