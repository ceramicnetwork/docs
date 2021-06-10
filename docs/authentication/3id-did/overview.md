# 3ID DID

3ID (CIP-79) is a DID method supported by Ceramic which can be used to authenticate and perform [writes](). 3IDs are the most suitable DID method for end users, but can also be used for any subject such as orgs, businesses, apps, devices, etc. 3ID is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

## **Usage**
Developers have two options for using 3ID in their application.

- Install a DID wallet that supports 3ID, such as [3ID Connect]()
- Install the low-level [3ID DID Provider]()

## **Benefits**
The primary bebefit of the 3ID DID Method is that it supports a **mutable DID document**. This is particularly important since it enables 3ID DIDs to handle secure key rotations and the addition and removal or arbitrary keys.

## **Tech Specs**
Below, find a simplified version of the 3ID specification. View the full [3ID DID Method (CIP-79) specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md).

### Method specfic identifier
`3`

Example:
```
did:3:<method-specific-identifier>
```

### DID Document
3ID offers a mutable DID document which can be used to set, remove, and rotate keys. This DID document is stored as a stream on Ceramic using the [TileDocument StreamType](). The DID document for a 3ID is stored natively on Ceramic as a Tile Document StreamType, allowing for mutability and enabling it to securely handle key rotations. This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies.

See [CIP-79](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) for the full technical specification of the 3ID DID method.

### DID Resolver
Implementation. This 3ID DID Resolver is included in Ceramic by default.


## **Underlying technology**

- [W3C DID specification]()
- [TileDocument StreamType (CIP-8)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md)

## **License**
3ID Connect is fully open sourced under MIT and Apache 2.

When 3IDs are used in conjunction with IDX and the 3ID Keychain (as is implemented in 3ID Connect), a 3ID can easily be controlled with any number of blockchain accounts from any L1 or L2 network. This provides a way to unify a user's identity across all other platforms.
