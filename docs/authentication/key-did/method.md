# Key DID

Key DID is a DID method supported by Ceramic which can be used to authenticate to Ceramic and perform [writes]() to streams that rely on DIDs for authentication. Key DIDs are a suitable DID method for advanced users who will only want to ever use one keypair to control their DID, and who have strong key security practices - such as a developer. The DID document for Key DID is immutable and therefore has no ability to rotate keys if it is compromised; it is explicitly tied to a single crypto key. Key DID is on the [W3C's official DID method registry]() and is fully compliant with decentralized identity standards.

## **Usage**
Developers have one option for using Key DIDs to authenticate to Ceramic.

- Install the low-level [Key DID Provider]() library

## **Benefits**
The primary benefit of the Key DID method is that it is lightweight, but this is also its main drawback.

## **Tech Specs**
Below, find a simplified version of the Key DID specification. View [the W3C specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) for the full Key DID method specification.

### Method name
The official method name for the 3ID DID method is `key`.

Example Key DID identifier:
```
did:key:<method-specific-identifier>
```

### DID Document
Key DID offers an immutable DID document that is statically generated from any cryptographic key pair. The DID document is not actually stored anywhere since it can always be regenerated from the key pair. Due 

### DID Resolver
Implementation. This Key DID Resolver is included in Ceramic by default.

## **Underlying technology**

- [W3C DID specification]()

## **License**
Key DID is fully open sourced under MIT and Apache 2.
