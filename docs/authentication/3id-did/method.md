# 3ID DID Method

The [3ID DID Method (CIP-79)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) is a [DID method](../../learn/glossary.md#did-methods) which can be used to [authenticate](../../build/authentication.md) to Ceramic and perform [writes](../../build/writes.md) to streams that rely on DIDs. 3ID DID is a powerful DID method that supports multiple keys, key rotations, and revocations. The 3ID DID Method is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

## **Considerations**

**Secure key rotations**: Because the [DID Document](../../learn/glossary.md#did-document) for the 3ID DID Method is implemented using a [TileDocument StreamType](../../streamtypes/tile-document/overview.md) on Ceramic, it is fully mutable and can support the secure addition and removal (rotation) of keys and other arbitrary data. This enables 3ID DIDs to handle multiple keys simultaneously and to remove keys when needed.

**Aggregated identities**: 3ID DIDs can serve as a cross-chain identifier which can be controlled by all of a user's blockchain and Web3 accounts from any L1 or L2 protocol. This provides a way to unify a user's identity across all other platforms.

**Seamless integration with blockchain wallets**: When 3ID DIDs are used in conjunction with [IDX (CIP-11)](../../tools/identity/idx.md) (using the [3ID Keychain definition (CIP-20)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-20/CIP-20.md)), a 3ID DID can be controlled from a user's existing blockchain wallets. This functionality is implemented by [3ID Connect](./3id-connect.md).

**Great for end users**: Due to all of the reasons above.


## **Installation**
To use the 3ID DID Method for authentication in your project, install one of the 3ID DID Providers below and be sure to include the 3ID DID Resolver in your project during [installation](../../build/installation.md).

### 3ID Connect
The most popular 3ID DID Provider for Ceramic web apps. The 3ID Connect SDK allows users to control their 3ID DID from their existing blockchain wallets without needing to install any additional software. Developers do not need to worry about DID key management for their users.

> [**Install 3ID Connect**](./3id-connect.md) (recommended)

### 3ID DID Provider
A low-level JavaScript 3ID DID Provider. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret.

> [**Install 3ID DID Provider**](./provider.md)

## **Specification**
The 3ID DID Method is fully implemented on top of Ceramic. Below, find a simplified version of the 3ID DID Method specification. For the full specification, view [3ID DID Method (CIP-79)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md).

### Method name
The official method name for the 3ID DID method is `3`.

### DID string
Example 3ID DID identifier:

```
did:3:<method-specific-identifier>
```

### DID Document
3ID offers a mutable DID document which can be used to securely set, remove, and rotate keys. This DID document is stored as a stream on Ceramic using the [TileDocument StreamType](../../streamtypes/tile-document/overview.md). This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies when using 3ID DIDs.

### DID Resolver
[3ID DID Resolver](./resolver.md) is a JavaScript DID resolver for the 3ID DID Method. It uses Ceramic to resolve DID documents.


## **Underlying technology**

- [W3C DID specification](https://www.w3.org/TR/did-core/)
- [TileDocument StreamType](../../streamtypes/tile-document/overview.md)


</br></br></br>
