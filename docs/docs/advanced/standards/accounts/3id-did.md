# **3ID DID Accounts**

The 3ID DID Method (CIP-79) is a Ceramic-native account which can be used to authenticate to Ceramic to perform transactions on streams. 



## **What is a 3ID account?**

---

3ID DID is a powerful DID method that supports multiple keys, key rotations, and revocations. The 3ID DID Method is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

- **Multiple keys, secure rotations**: The DID Document for the 3ID DID Method is implemented using a Ceramic [Tile Document](../stream-programs/tile-document.md), making it fully mutable and able to support the secure addition and removal (rotation) of keys. This enables 3ID DIDs to handle multiple keys simultaneously and to remove keys when needed.
- **Support for blockchain wallets**: When 3ID DIDs are used in conjunction with [the Identity Index protocol (CIP-11)](../application-protocols/identity-index.md) (using the [3ID Keychain definition (CIP-20)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-20/CIP-20.md)), a 3ID DID can be controlled from a user's existing blockchain wallets. This functionality is implemented by [3ID Connect](../../../../reference/accounts/3id-did.md#3id-connect).
- **Aggregated identities**: 3ID DIDs can serve as a cross-chain identifier which can be controlled by all of a user's blockchain and Web3 accounts from any L1 or L2 protocol. This provides a way to unify a user's identity across all other platforms.
- **Great for end users**: Due to all of the reasons above.

## **Specification**

---

3ID DIDs are fully implemented on top of Ceramic. For the full specification, see [3ID DID Method (CIP-79)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md).


### **Account identifier**

```
did:3:<StreamID of the TileDocument storing the DID Document>
```

### **DID Document**

3ID offers a mutable DID document which can be used to securely set, remove, and rotate keys. This DID document is stored as a stream on Ceramic using the [TileDocument StreamType](../stream-programs/tile-document.md).

## **Building with 3ID DID**

---

To use 3ID DIDs for user accounts in your project, install one of the 3ID DID Providers below:

### [**Install 3ID Connect →**](../../../../reference/accounts/3id-did.md#3id-connect)

**Recommended.** 3ID Connect is the most popular 3ID DID Provider for Ceramic web apps. The 3ID Connect SDK allows users to control their 3ID DID from their existing blockchain wallets without needing to install any additional wallet software. Developers do not need to worry about DID key management for their users.

### [**Install 3ID DID Provider →**](../../../../reference/accounts/3id-did.md#3id-did-provider)

A low-level JavaScript 3ID DID Provider. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret.

### [**Install DID Resolver →**](../../../../reference/accounts/3id-did.md#3id-did-resolver)

3ID DID Resolver is a JavaScript DID resolver for the 3ID DID Method. It uses Ceramic to resolve DID documents.

## **Additional reading**

---

- [3ID DID Method specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md)
- [W3C DID specification](https://www.w3.org/TR/did-core/)
- [TileDocument StreamType](../stream-programs/tile-document.md)
