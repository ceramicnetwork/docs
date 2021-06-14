# Key DID Method

Key DID is a [DID method](../../learn/glossary.md#did-methods) supported by Ceramic which can be used to [authenticate](../../build/authentication.md) to a Ceramic client to perform [writes]() to streams that rely on DIDs for authentication. The Key DID Method is the most simple DID method. It simply encodes a public key in the DID string, and when resolved converts this public key into a [DID Document](../../learn/glossary.md#did-document). Key DID is on the W3C's [official DID method registry]() and is fully compliant with decentralized identity standards. Carefully read the considerations below before deciding to use the Key DID Method in your project.

## **Considerations**

**One key only**: The DID Document for a Key DID is explicitly tied to a single crypto key. It can not support multiple keys in the DID document, which means only one key can ever control the DID and it can never be changed in case it is compromised.

**For advanced users**: For the reasons above, the Key DID Method is only suitable for advanced users who will only want to ever use one keypair to control their DID, and who have strong key security practices - such as a developer. 

## **Usage**
There is one available Key DID Provider for use with Ceramic clients:

- [Key DID Provider Ed25519](./provider.md)

## **Specification**
Below, find a simplified version of the Key DID specification. View the [complete W3C specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md).

### Method name
The official method name for the Key DID method is `key`.

### DID string
The DID string for the Key DID Method simply encodes the public key. Example Key DID identifier:

```
did:key:<public-key>
```

### DID Document
Key DID offers an immutable DID document that is statically generated from any cryptographic key pair. The DID document is not actually stored anywhere since it can always be regenerated from the key pair.

### DID Resolver
The [Key DID Resolver](./reslover.md) takes a Key DID string and returns a DID Document that includes the key pair.

## **Underlying technology**

- [W3C DID specification]()
- [did:key specification](https://w3c-ccg.github.io/did-method-key/){:target="_blank"}

</br></br></br>