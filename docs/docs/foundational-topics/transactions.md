# Transactions

Transactions are cryptographically signed instructions from accounts. An account will initiate a transaction to update the state of a stream on the Ceramic network. The simplest transaction is creating a new stream.

- Transaction Format: index.md
- Create a Stream: index.md
- Change a Stream: index.md
- Change Multiple Streams: index.md
- Perform Bulk Transactions: index.md
- Transaction Retries: index.md
- Proxy Transactions: index.md

## What's a transaction?

---

A Ceramic transaction refers to an action initiated by an account. For example, if Alice wants to update a stream that she controls, she would send a transaction addressed to that stream specifically.

Diagram showing a transaction cause state change
Diagram adapted from Ethereum EVM illustrated

Transactions, which change the state of a stream on Ceramic, are first applied to the stream locally on a single node, then broadcast to the whole network. After this happens, other nodes that are storing that stream will receive the message,execute the transaction, and update the stream's state local to their node. This is how state updates propagate throughout the network of all nodes.

After this process, transactions are only considered pending. Transactions must subsequently be finalized by being anchored on a blockchain, such as Ethereum. Ceramic nodes forward valid transactions to Anchor nodes, which collect updates from many nodes during a given interval, create a merkle tree of these tranactions, and include the merkle root in transaction calldata on Ethereum.

## Transaction Format

---

A submitted transaction includes the following information:

| Field | Required | Description | 
| --- | --- | --- |
| `streamID` | N | Recipient stream for the transaction. If empty, creates a new stream. |
| `content` | Y | Accounts allowed to send transactions. |
| `schema` | N | JSON schema used for the content. |
| `family` | N | Arbitrary string. |
| `tags` | N | Array of arbitrary strings. |

The transaction object will look a little like this:

``` json
{
  from: "0xEA674fdDe714fd979de3EdF0F56AA9716B898ec8",
  to: "0xac03bb73b6a9e108530aff4df5077c2b3d481e5a",
  gasLimit: "21000",
  maxFeePerGas: "300",
  maxPriorityFeePerGas: "10",
  nonce: "0",
  value: "10000000000"
}
```

But a transaction object needs to be signed using the sender's private key. This proves that the transaction could only have come from the sender and was not sent fraudulently.

A wallet library like 3ID.js will handle this signing process.

Example JSON-RPC call:

``` json
{
  "id": 2,
  "jsonrpc": "2.0",
  "method": "account_signTransaction",
  "params": [
    {
      "from": "0x1923f626bb8dc025849e00f99c25fe2b2f7fb0db",
      "gas": "0x55555",
      "maxFeePerGas": "0x1234",
      "maxPriorityFeePerGas": "0x1234",
      "input": "0xabcd",
      "nonce": "0x0",
      "to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
      "value": "0x1234"
    }
  ]
}
```

Example response:

``` json
{
  "jsonrpc": "2.0",
  "id": 2,
  "result": {
    "raw": "0xf88380018203339407a565b7ed7d7a678680a4c162885bedbb695fe080a44401a6e4000000000000000000000000000000000000000000000000000000000000001226a0223a7c9bcf5531c99be5ea7082183816eb20cfe0bbc322e97cc5c7f71ab8b20ea02aadee6b34b45bb15bc42d9c09de4a6754e7000908da72d48cc7704971491663",
    "tx": {
      "nonce": "0x0",
      "maxFeePerGas": "0x1234",
      "maxPriorityFeePerGas": "0x1234",
      "gas": "0x55555",
      "to": "0x07a565b7ed7d7a678680a4c162885bedbb695fe0",
      "value": "0x1234",
      "input": "0xabcd",
      "v": "0x26",
      "r": "0x223a7c9bcf5531c99be5ea7082183816eb20cfe0bbc322e97cc5c7f71ab8b20e",
      "s": "0x2aadee6b34b45bb15bc42d9c09de4a6754e7000908da72d48cc7704971491663",
      "hash": "0xeba2df809e7a612a0a0d444ccfa5c839624bdc00dd29e3340d46df3870f8a30e"
    }
  }
}
```

the raw is the signed transaction in Recursive Length Prefix (RLP) encoded form
the tx is the signed transaction in JSON form
With the signature hash, the transaction can be cryptographically proven that it came from the sender and submitted to the network.

## Transaction Lifecycle

---

Once the transaction has been submitted the following happens:

Once you send a transaction, it gets published to IPLD which returns a cryptographic hash that identifies the content: 0x97d99bc7729211111a21b12c933c949d4f31684f1d6954ff477d0477538ff017
The transaction is then broadcast to the network and included in a pool with lots of other transactions.
A miner must pick your transaction and include it in a block in order to verify the transaction and consider it "successful".
You may end up waiting at this stage if the network is busy and miners aren't able to keep up.
Your transaction will receive "confirmations". The number of confirmations is the number of blocks created since the block that included your transaction. The higher the number, the greater the certainty that the network processed and recognised the transaction.
Recent blocks may get re-organised, giving the impression the transaction was unsuccessful; however, the transaction may still be valid but included in a different block.
The probability of a re-organisation diminishes with every subsequent block mined, i.e. the greater the number of confirmations, the more immutable the transaction is.
A VISUAL DEMO
Watch Austin walk you through transactions, gas, and mining.

## Further reading

---

- EIP-2718: Typed Transaction Envelope

## Related topics

---

- Ethereum virtual machine (EVM)
- Gas
- Mining