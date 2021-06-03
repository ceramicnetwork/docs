# Anchor Service (CAS)
The Ceramic Anchor Service (CAS) is a hosted layer 2 solution for batching many Ceramic transactions
into a single blockchain transaction.

A vanilla [Merkle-DAG-based](https://docs.ipfs.io/concepts/merkle-dag/) system has no notion of absolute time. 
Ceramic employs a blockchain to provide tamper-proof timestamps.

Ceramic anchor service combines anchor requests (containing a [StreamID](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_streamid.streamid-1.html) and a [CommitID](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_streamid.commitid-1.html)) into a [Merkle Tree](https://en.wikipedia.org/wiki/Merkle_tree).
Then it pushes a transaction containing root of the composed tree to a blockchain. After a transaction makes its way onto a blockchain,
a Ceramic node creates a special anchor commit that references the blockchain transaction. One could deduce thus
when one created the commit, based on a block time of the transaction.

Ceramic network has few separate networks: `dev-unstable`, `testnet-clay`, and `mainnet`. 3Box Labs hosts an instance of Ceramic Service for each network.
`dev-unstable` and `testnet-clay` use Ethereum Rinkeby or Ethereum Ropsten for anchoring. Transactions are free here, so the anchor services are freely available.
`mainnet` anchor service on the other hand uses main Ethereum network. For some time the mainnet anchor service is only available to early-launch partners because of DoS concerns.
When Ceramic network operates in a _fully_ decentralized way, one could run own mainnet anchor service.
For now one should rely on the hosted anchor services only.

For more details about the JavaScript CAS implementation, refer to its repo:

[:octicons-file-code-16: Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service){:target="_blank"}
