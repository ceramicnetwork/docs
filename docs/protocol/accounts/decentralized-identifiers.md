# Identifiers

Ceramic streams rely on an account model to authenticate and authorize updates to a stream. A fully realized vision of user owned data includes the use of public key cryptography and the ability to sign data with a public-private key-pair controlled by a user. But key pairs alone are often not user friendly nor sufficient and don't fully represent the range of real world scenarios. 

## Decentralized Identifiers (DIDs)

---

Ceramic uses [Decentralized Identifiers (DIDs)](https://w3c.github.io/did-core/) to represent accounts. DIDs are identifiers that enable verifiable, decentralized digital identities. They require no centralized party or registry and are extremely extensible, allowing a variety of implementations and account models to exist. 

DID methods are specific implementations of the DID standard that define an identifier namespace along with how to resolve its DID document, which typically stores public keys for signing and encryption. The ability to resolve public keys from identifiers allows anyone to verify a signature for a DID. 

## Supported Methods

---

At this time, the following DID methods can be used with Ceramic: 

### PKH DID

**PKH DID Method**: A DID method that natively supports blockchain accounts. DID documents are statically generated from a blockchain account, allowing blockchain accounts to sign, authorize and authenticate in DID based environments. PKH DID is the primary and recommended method in Ceramic. [did:pkh Method Specification](https://github.com/w3c-ccg/did-pkh/blob/main/did-pkh-method-draft.md)

```html
did:pkh:eip155:1:0xb9c5714089478a327f09197987f16f9e5d936e8a
```

### Key DID

**Key DID Method**: A DID method that expands a cryptographic public key into a DID Document, with support for Ed25519 and Secp256k1. Key DIDs are typically not used in long lived environments. [did:key Method Specification](https://w3c-ccg.github.io/did-method-key/)

```html
did:key:z6MkhaXgBZDvotDkL5257faiztiGiC2QtKLGpbnnEGta2doK
```

### 3ID DID

**3ID DID Method**: A DID method that uses Ceramic's Tile Document StreamType to represent a mutable DID document. 3ID can be controlled with any number of blockchain accounts. [did:3id Method Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md)

```html
did:3:kjzl6cwe1jw149tlplc4bgnpn1v4uwk9rg9jkvijx0u0zmfa97t69dnqibqa2as
```