# 3ID DID Method

3ID DID (CIP-79) is a [DID method](../../learn/glossary.md#did-methods) which can be used to [authenticate](../../build/authentication.md) to Ceramic and perform [writes]() to streams that rely on DIDs for authentication. 3IDs are the most suitable DID method for end users, but can also be used for any subject such as orgs, businesses, apps, devices, etc. The 3ID DID Method is on the W3C's [official DID method registry]() and is fully compliant with decentralized identity standards.

## **Considerations**

**Mutable DID document**: The 3ID DID Method is fully implemented on top of Ceramic. The DID Document for 3ID DIDs is stored in a [TileDocument StreamType](../../streamtypes/tile-document/overview.md). The primary benefit of the 3ID DID method is that it has a **mutable DID document**. This is particularly important since it enables 3ID DIDs to handle secure key rotations and the addition and removal or arbitrary data.

**Multi-account identities**: When 3ID DIDs are used in conjunction with [IDX (CIP-11)](../../tools/identity/idx.md) (using the [3ID Keychain definition (CIP-20)](), a 3ID DID can be controlled with any number of blockchain wallets from any L1 or L2 protocol. This provides a way to unify a user's identity across all other platforms. This functionality is implemented by [3ID Connect](./3id-connect.md).


## **Usage**
To use the 3ID DID Method for authentication in your application, you need to install a 3ID DID Provider and be sure to include the 3ID DID Resolver in your project when setting up your client.

- [3ID Connect Provider](./3id-connect.md) (recommended)
- [3ID DID Provider](./provider.md)

## **Specification**
Below, find a simplified version of the 3ID specification. View [CIP-79](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) for the full 3ID DID method specification.

### Method name
The official method name for the 3ID DID method is `3`.

### DID string
Example 3ID DID identifier:

```
did:3:<method-specific-identifier>
```

### DID Document
3ID offers a mutable DID document which can be used to securely set, remove, and rotate keys. This DID document is stored as a stream on Ceramic using the [TileDocument StreamType](). This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies when using 3ID DIDs.

### DID Resolver
Implementation. This 3ID DID Resolver is included in Ceramic by default.


## **Underlying technology**

- [W3C DID specification]()
- [TileDocument StreamType (CIP-8)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md)

## **License**
3ID Connect is fully open sourced under MIT and Apache 2.

</br></br></br>
