# 3ID DID Method

The [3ID DID Method (CIP-79)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) is a [DID method](../../../learn/glossary.md#did-methods) which can be used to [authenticate](../../../build/javascript/authentication.md) to Ceramic and perform [writes](../../../build/javascript/writes.md) to streams that rely on DIDs. 3ID DID is a powerful DID method that supports multiple keys, key rotations, and revocations. The 3ID DID Method is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

## What is a 3ID account?

---

- **Secure key rotations**: Because the [DID Document](../../../learn/glossary.md#did-document) for the 3ID DID Method is implemented using a [TileDocument StreamType](../../../streamtypes/tile-document/overview.md) on Ceramic, it is fully mutable and can support the secure addition and removal (rotation) of keys and other arbitrary data. This enables 3ID DIDs to handle multiple keys simultaneously and to remove keys when needed.
- **Seamless integration with blockchain wallets**: When 3ID DIDs are used in conjunction with [the Identity Index protocol (CIP-11)](../application-protocols/cip11-identity-index.md) (using the [3ID Keychain definition (CIP-20)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-20/CIP-20.md)), a 3ID DID can be controlled from a user's existing blockchain wallets. This functionality is implemented by [3ID Connect](../../../reference/accounts/3id-did.md#3id-connect).
- **Aggregated identities**: 3ID DIDs can serve as a cross-chain identifier which can be controlled by all of a user's blockchain and Web3 accounts from any L1 or L2 protocol. This provides a way to unify a user's identity across all other platforms.
- **Great for end users**: Due to all of the reasons above.

## Specification

---

The 3ID DID Method is fully implemented on top of Ceramic. Below, find a simplified version of the 3ID DID Method specification. For the full specification, view [3ID DID Method (CIP-79)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md).

The official method name for the 3ID DID method is `3`.

| CIP     | NAME                 | EXAMPLE                                                                                                               |
| ------- | -------------------- | --------------------------------------------------------------------------------------------------------------------- |
| Name    | `3`                  | A standard interface for fungible (interchangeable) tokens, like voting tokens, staking tokens or virtual currencies. |
| URI     | Identifier           | `did:3:<StreamID>`                                                                                                    |
| Storage | Ceramic TileDocument | 3ID offers a mutable DID document which can be used to securely set, remove, and rotate keys.                         |

### DID string

Example 3ID DID identifier:

```
did:3:<StreamID of the Ceramic TileDocument which contains the DID Document>
```

### DID Document

3ID offers a mutable DID document which can be used to securely set, remove, and rotate keys. This DID document is stored as a stream on Ceramic using the [TileDocument StreamType](../../../streamtypes/tile-document/overview.md). This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies when using 3ID DIDs.

## Libraries

---

To use the 3ID DID Method for authentication in your project, install one of the 3ID DID Providers below and be sure to include the 3ID DID Resolver in your project during [installation](../../../build/javascript/installation.md).

### 3ID Connect

The most popular 3ID DID Provider for Ceramic web apps. The 3ID Connect SDK allows users to control their 3ID DID from their existing blockchain wallets without needing to install any additional software. Developers do not need to worry about DID key management for their users.

[Install](../../../reference/accounts/3id-did.md#3id-connect){: .md-button .md-button--primary } (recommended)

### 3ID DID Provider

A low-level JavaScript 3ID DID Provider. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret.

[Install](../../../reference/accounts/3id-did.md#3id-did-provider){: .md-button .md-button--primary }

### DID Resolver

[3ID DID Resolver](../../../reference/accounts/3id-did.md#3id-did-resolver) is a JavaScript DID resolver for the 3ID DID Method. It uses Ceramic to resolve DID documents.

## **Underlying technology**

- [3ID DID Method specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md)
- [W3C DID specification](https://www.w3.org/TR/did-core/)
- [TileDocument StreamType](../../../streamtypes/tile-document/overview.md)
