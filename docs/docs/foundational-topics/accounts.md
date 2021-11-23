# Ceramic Accounts

A Ceramic account is an entity that can send transactions on Ceramic. Accounts can be user-controlled or deployed as smart contracts.

## Prerequisites

Accounts are a very beginner-friendly topic. But to help you better understand this page, we recommend you first read through our introduction to Ethereum.

## Account types

Ceramic has three account types:

- Externally-owned – controlled by anyone with the private keys

- Smart contract: smart contract deployed to the network, controlled by code. Learn about smart contracts

    - DAO – a smart contract deployed to the network, controlled by code. Learn about smart contracts
    - NFT – a smart contract deployed to the network, controlled by code. Learn about smart contracts

All account types have the ability to:

- Receive, hold and send ETH and tokens
- Interact with deployed smart contracts


### Key differences

#### Externally-owned

- Creating an account costs nothing
- Can initiate transactions
- Transactions between externally-owned accounts can only be ETH/token transfers

#### Contract

Creating a contract has a cost because you're using network storage
Can only send transactions in response to receiving a transaction
Transactions from an external account to a contract account can trigger code which can execute many different actions, such as transferring tokens or even creating a new contract

## The anatomy of an account

Ceramic accounts have four fields:

- nonce – a counter that indicates the number of transactions sent from the account. This ensures transactions are only processed once. In a contract account, this number represents the number of contracts created by the account
- balance – the number of wei owned by this address. Wei is a denomination of ETH and there are 1e+18 wei per ETH.
- codeHash – this hash refers to the code of an account on the Ethereum virtual machine (EVM). Contract accounts have code fragments programmed in that can perform different operations. This EVM code gets executed if the account gets a message call. It cannot be changed unlike the other account fields. All such code fragments are contained in the state database under their corresponding hashes for later retrieval. This hash value is known as a codeHash. For externally owned accounts, the codeHash field is the hash of an empty string.
- storageRoot – Sometimes known as a storage hash. A 256-bit hash of the root node of a Merkle Patricia trie that encodes the storage contents of the account (a mapping between 256-bit integer values), encoded into the trie as a mapping from the Keccak 256-bit hash of the 256-bit integer keys to the RLP-encoded 256-bit integer values. This trie encodes the hash of the storage contents of this account, and is empty by default.

A diagram showing the make up of an account.

## Externally-owned accounts and key pairs

An account is made up of a cryptographic pair of keys: public and private. They help prove that a transaction was actually signed by the sender and prevent forgeries. Your private key is what you use to sign transactions, so it grants you custody over the funds associated with your account. You never really hold cryptocurrency, you hold private keys – the funds are always on Ethereum's ledger.

This prevents malicious actors from broadcasting fake transactions because you can always verify the sender of a transaction.

If Alice wants to send ether from her own account to Bob’s account, Alice needs to create a transaction request and send it out to the network for verification. Ethereum’s usage of public-key cryptography ensures that Alice can prove that she originally initiated the transaction request. Without cryptographic mechanisms, a malicious adversary Eve could simply publicly broadcast a request that looks something like “send 5 ETH from Alice’s account to Eve’s account,” and no one would be able to verify that it didn’t come from Alice.

## Account creation

When you want to create an account most libraries will generate you a random private key.

A private key is made up of 32 byte Uint8Array and can be encrypted with a password.

Example:

```
fffffffffffffffffffffffffffffffebaaedce6af48a03bbfd25e8cd036415f
```

The public key is generated from the private key using the Elliptic Curve Digital Signature Algorithm. You get a public address for your account by taking the last 20 bytes of the Keccak-256 hash of the public key and adding 0x to the beginning.

Here's an example of creating an account in the console using GETH's personal_newAccount

> personal.newAccount()
Passphrase:
Repeat passphrase:
"0x5e97870f263700f46aa00d967821199b9bc5a120"

> personal.newAccount("h4ck3r")
"0x3d80b31a78c30fc628f20b2c89d7ddbf6e53cedc"

GETH documentation

It is possible to derive new public keys from your private key but you cannot derive a private key from public keys. This means it's vital to keep a private key safe and, as the name suggests, PRIVATE.

You need a private key to sign messages and transactions which output a signature. Others can then take the signature to derive your public key, proving the author of the message. In your application, you can use a javascript library to send transactions to the network.

CONTRACT ACCOUNTS
Contract accounts also have a 42 character hexadecimal address:

Example:

0x06012c8cf97bead5deae237070f9587f8e7a266d

The contract address is usually given when a contract is deployed to the Ethereum Blockchain. The address comes from the creator's address and the number of transactions sent from that address (the “nonce”).

## Account abstraction
Ceramic uses the decentralzied identifier protocol (DIDs), which is an interoperability protocol for accounts, to provide an abstract interface which allows application code to interact with accounts in a unified manner, regardless of the type.

## A note on wallets
An account is not a wallet. A wallet is the keypair associated with a user-owned account, which allows a user to make transactions from or manage the account.

## A viual demo
Watch Gabe walk you through hash functions, and key pairs.

```
.
.
.
.
.
.
.
.
.
.
.
.
.
```

## Further Reading
Know of a community resource that helped you? Edit this page and add it!

## Related Topics

- Smart contracts
- Transactions


---
---
---
# Ceramic Accounts

A Ceramic account is an entity that owns streams on Ceramic and can send transactions to those streams. Accounts can be xxx, xxx, or xxxx, which all can be represented by decentralized identifiers.

![](../../images/verse.png)

On Ceramic, accounts are how streams are owned and permissioned throughout the Ceramic network. You can think of a Ceramic account as a folder that holds a number of public key-pair accounts as authorized signers. Accounts on Ceramic serve as durable user (read: not necessarily human) identifies that are used to authenticate transactions (read: verify signatures) The way that these signers are managed defines the account implementation. Sometimes. Add some more details on the role of an account.

---

##### Things to Know
A couple of things to know about Ceramic's account system:

- For accounts, Ceramic uses the [Decentralized Identifiers (DIDs) Protocol]() which has been developed in conjunction with the Decentralized Identity Foundation (DIF) and the W3C.
- There are over 80 account methods that conform to the DID protocol.
- Ceramic can support any DID as an account, see [Adding Support for a New Account Type]().
- There is not one "account" type on ceramic.
- Ceramic uses DIDs as public key infrastructure for its account system.
- DIDs separate the concepts of accounts and signers.
- An account is analogous to a folder that can contain one or more signers.
- Accounts can own states in the protocol.
- Signers can sign transactions on behalf of the account.
- If it can be represented by a DID it can have an account on Ceramic.
- Signers can be accounts from existing Web3 wallets, from any chain.

---

## Account Types
Really implied based on choice above. Ceramic does not really have the concept of wallets since Ceramic accounts do not hold value. Ceramic has many different models of accounts, each with their unique strengths and weaknesses. You may need to ensure your node has support for more specialized DIDs, etc. The first decision is how you want the users of your application to sign transactions, which really depends on your. You should choose. To invoke during the authenticate method of the DID API. Providing Ceramic API authentication, transaction signature and authentication and permissions. Really comes down to the relationship you want between your Ceramic account and its signing accounts.

Ceramic currently supports three account types, which offer different solutions for. All account types have the ability to create, own, and send transactions to streams.

##### Owned Accounts

| Type | Name | Infrastructure      | Description |
| --- | -------- | ----------- | ----------- |
| Fixed | Key | Public Key      | Title       |
| 3ID | Streams   | Text        |
| NFT, Gnosis Safe | Smart Contract   | Text        |

##### Contract Accounts

| Type | Name | Infrastructure      | Description |
| --- | -------- | ----------- | ----------- |
| Fixed | Key | Public Key      | Title       |
| Gnosis Safe | Streams   | Text        |
| NFT, Gnosis Safe | Smart Contract   | Text        |

---

## Account Design
Ceramic accounts have a few key concepts, which follow the DID specificaton, but are illustrated here for clarity:

- Identifier

- Document

- Infrastructure

---

## Account Libraries
Every account type listed on this page requires three pieces of software:

- Universal Resolver:
- Method-specific provider:
- Method-specific resolver:

---

## Wallets

---

## Additional Resources

### Further Reading

### Related Topics

---
