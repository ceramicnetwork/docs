# **Key DID libraries**

---

The Key DID libraries include the [resolver](#key-did-resolver) and [multiple providers](#key-did-providers) to provide a simple way for developers to get started using the [DID client](../core-clients/did-jsonrpc.md) with the `did:key` method.

## **Available libraries**

---

- The [Key DID resolver](#key-did-resolver) allows a DID JSON-RPC client to resolve accounts using the `did:key` method
- The [Key DID provider ED25519](#key-did-provider-ed25519) allows applications to create and use Key DID accounts for ED25519 keypairs. This provider supports encryption.
- The [Key DID provider secp256k1](#key-did-provider-secp256k1) allows applications to create and use Key DID accounts for secp256k1 keypairs. This provider does not supports encryption.

## **Key DID resolver**

---

The `key-did-resolver` module is needed to resolve DID documents using the `did:key` method.

### **Installation**

```sh
npm install key-did-resolver
```

### **Usage**

```ts
import { DID } from 'dids'
import { getResolver } from 'key-did-resolver'

async function resolveDID() {
  const did = new DID({ resolver: getResolver() })
  return await did.resolve('did:key:...')
}
```

## **Key DID providers**

---

Different libraries implement a provider for the `did:key` method based on different cryptographic primitives. These providers may have different possibilities, for example `key-did-provider-ed25519` supports encryption while `key-did-provider-secp256k1` does not.

## **Key DID provider ED25519**

---

This is the **recommended provider** for the `key:did` method in most cases.

### **Installation**

```sh
npm install key-did-provider-ed25519
```

### **Usage**

```ts
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

// `seed` must be a 32-byte long Uint8Array
async function authenticateDID(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  await did.authenticate()
  return did
}
```

## **Key DID provider secp256k1**

---

This provider *does not support encryption*, so using methods such as `createJWE` on the `DID` instance is not supported.

### **Installation**

```sh
npm install key-did-provider-secp256k1
```

### **Usage**

```ts
import { DID } from 'dids'
import { Secp256k1Provider } from 'key-did-provider-secp256k1'
import { getResolver } from 'key-did-resolver'

// `seed` must be a 32-byte long Uint8Array
async function authenticateDID(seed) {
  const provider = new Secp256k1Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  await did.authenticate()
  return did
}
```
