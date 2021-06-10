# Key DID Provider
Key DID Provider is a low-level JavaScript interface for creating and interacting with a Key DID. View [Overview]() for a more general description of the Key DID Method.

## **Installation**

### 1. Install from npm

``` sh
$ npm install key-did-provider-ed25519
```

### 2. Import the Provider

``` javascript
import { Ed25519Provider } from 'key-did-provider-ed25519'
```

### 3. Get seed for DID

Generate a random seed for a new user, or somehow get the existing seed for a returning user. Seeds should be a 32 byte Uint8Array. Here's how to securely generate a seed in the proper format:

``` javascript
import { randomBytes } from '@stablelib/random'
const seed = randomBytes(32)
```

### 4. Create a provider instance using the seed

``` js
const provider = new Ed25519Provider(seed)
```

### 5. Set the Provider to Ceramic

Set the Provider instance to the DID instance used by your Ceramic client in order to perform writes.
``` javascript
ceramic.did.setProvider(provider)
```

### 6. Authenticate the Key DID
Now all that's left is to authenticate to the Ceramic client's DID instance using the configured Key DID Provider.
``` js
await ceramic.did.authenticate()
```

## **Usage**
After authenticating, the user will now be able to perform [writes](writes.md) on Ceramic using their DID.

## **License**
Key DID Provides is fully open sourced under MIT and Apache 2.

</br>
</br>
</br>
