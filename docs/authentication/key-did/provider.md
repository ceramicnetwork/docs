# Key DID Provider Ed25519

Key DID Provider Ed25519 is a [DID Provider](../../learn/glossary.md#did-provider) for the [Key DID Method](./method.md) that works with `Ed25519` key pairs. It allows you to create and use Key DIDs within your application.

## **Installation**

Before installing Key DID Provider Ed25519, you must have [installed a Ceramic client](../../build/javascript/installation.md). By following the steps below, users will be able to [perform writes](../../build/javascript/writes.md) on Ceramic using a Key DID.

### 1. Install from npm

```sh
$ npm install key-did-provider-ed25519
```

### 2. Import the Provider

```javascript
import { Ed25519Provider } from 'key-did-provider-ed25519'
```

### 3. Get seed for DID

Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array. Here's how to securely generate a seed in the proper format:

```javascript
import { randomBytes } from '@stablelib/random'
const seed = randomBytes(32)
```

### 4. Create a provider instance using the seed

```js
const provider = new Ed25519Provider(seed)
```

### 5. Set the Provider to Ceramic

Set the Provider instance to the DID instance used by your Ceramic client in order to perform writes. You should have configured the DID instance when you [installed your client](../../build/javascript/installation.md).

```javascript
ceramic.did.setProvider(provider)
```

### 6. Authenticate the Key DID

Now all that's left is to authenticate to the Ceramic client's DID instance using the configured Key DID Provider.

```js
await ceramic.did.authenticate()
```

## **Next steps: Writes**

After authenticating with the 3ID DID Provider, users will now be able to perform [writes](../../build/javascript/writes.md).
