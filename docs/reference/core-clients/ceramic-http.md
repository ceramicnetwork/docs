# Ceramic.js

Ceramic.js is a library used to connect your web application to a Ceramic node. It can be used in both browser and Node.js environments. It is actively maintained and supports the latest Ceramic features.

![](../../images/verse.png)

##### Things to Know

- Thing 1
- Thing 2
- Thing 3

## Install Ceramic.js

---

First, install Ceramic.js from npm:

```bash
npm install @ceramicnetwork/http-client
```

Then, include Ceramic.js in your project:

```ts
// Add Ceramic.js
import CeramicClient from '@ceramicnetwork/http-client'

// Connect to a Ceramic node
const API_URL = 'https://your-ceramic-node.com'

// Create the Ceramic object
const ceramic = new CeramicClient(API_URL)
```

!!! note ""

    With this setup, your application will now be able to perform queries.

## Enable Transactions

---

Finally, set the DID object on the Ceramic object:

```ts
ceramic.did = did
```

!!! note ""

    With this setup, your application will now be able to perform queries.

## Wallets

---

## Example

---

## API Usage

---

### Query a stream

```ts
.
.
.
```

### Query a previous version

```ts
.
.
.
```

### Query a stream

```ts
.
.
.
```

### Query a stream

```ts
.
.
.
```

## Next Steps

---

- View all available API methods in the [Ceramic.js Reference]().
