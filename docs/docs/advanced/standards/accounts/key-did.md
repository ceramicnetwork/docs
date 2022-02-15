# **Key DID Accounts**

The Key DID Method is an account which can be used to authenticate to Ceramic to perform transactions on streams. 



## **What is a Key account?**

---

The Key DID Method is the most simple DID method. It simply encodes a public key in the DID string, and when resolved converts this public key into a [DID Document](../../../../learn/glossary.md#did-document). Key DID is on the W3C's official DID method registry and is fully compliant with decentralized identity standards. Carefully read the considerations below before deciding to use the Key DID Method in your project.

- **One key only**: The DID Document for a Key DID is explicitly tied to a single crypto key. It can not support multiple keys in the DID document nor can it support key rotation, which means only one key can ever control the DID and it can never be changed in case it is compromised.
- **For advanced users**: For the reasons above, the Key DID Method is only suitable for advanced users who will only want to ever use one keypair to control their DID, and who have strong key security practices - such as a developer - and so generally will not be appropriate for identities for non-technical end users.

## **Specification**

---

For the full specification, see [Key DID Method specification →](https://w3c-ccg.github.io/did-method-key/).

### **Account identifier**

The DID string for the Key DID Method simply encodes the public key. Example Key DID identifier:

```
did:key:<public-key>
```

### **DID Document**

Key DID offers an immutable DID document that is statically generated from any cryptographic key pair. The DID document is not actually stored anywhere since it can always be regenerated from the key pair.


## **Building with Key DID**

---

To use Key DIDs for user accounts in your project, install one of the Key DID Providers below:

### [**Install Key DID Provider Ed25519 →**](../../../../reference/accounts/key-did.md#ed25519)

A low-level JavaScript Key DID Provider for use with Ed25519 key pairs. Your application is responsible for key managemet, and users need to authenticate with a DID seed.

### [**Install Key DID Resolver →**](../../../../reference/accounts/key-did.md#key-did-resolver)

The Key DID Resolver is a JavaScript DID resolver for the Key DID Method.


## **Additional reading**

---

- [W3C DID specification](https://www.w3.org/TR/did-core/)
- [did:key specification](https://w3c-ccg.github.io/did-method-key/){:target="\_blank"}