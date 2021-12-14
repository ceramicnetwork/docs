# Ceramic HTTP client

The Ceramic HTTP client library can be used in browsers and Node.js to connect your application to a Ceramic node. It is actively maintained and supports the latest Ceramic features.

![](../../images/verse.png)

## Things to know

- The HTTP client currently supports loading [CAIP-10 links](../stream-programs/caip10-link.md) and [Tile document](../stream-programs/tile-document.md) streams.
- The client is read-only by default, to enable transactions a [DID client](did-jsonrpc.md) needs to be attached to the Ceramic client instance.
- Ceramic streams can be identified by a **stream ID** or a **commit ID**. A **stream ID** is generated when creating the stream and can be used to load the **latest version** of the stream, while a **commit ID** represents a **specific version** of the stream.

## Installation

```sh
npm install @ceramicnetwork/http-client
```

## Common options

TODO: describe options common to multiple methods and stream programs

### `anchor`

### `pin`

### `publish`

## Common use-cases

### Load a single stream

```ts
// Import the client
import { CeramicClient } from '@ceramicnetwork/http-client'

// Connect to a Ceramic node
const ceramic = new CeramicClient('https://your-ceramic-node.com')

// The `id` argument can be a stream ID (to load the latest version)
// or a commit ID (to load a specific version)
async function load(id) {
  return await ceramic.loadStream(id)
}
```

### Load multiple streams

Rather than using the `loadStream` method multiple times with `Promise.all()` to load multiple streams at once, a **more efficient way for loading multiple streams** is to use the `multiQuery` method.

```ts
// Import the client
import { CeramicClient } from '@ceramicnetwork/http-client'

// Connect to a Ceramic node
const ceramic = new CeramicClient('https://your-ceramic-node.com')

// The `ids` argument can contain an arry of stream IDs (to load the latest version)
// or commit IDs (to load a specific version)
async function loadMulti(ids = []) {
  const queries = ids.map((streamId) => ({ streamId }))
  // This will return an Object of stream ID keys to stream values
  return await ceramic.multiQuery(queries)
}
```

### Enable transactions

In order to create and update streams, the Ceramic client instance must be able to sign transaction payloads by using an authenticated DID instance. The [DID client documentation](did-jsonrpc.md) describes the process of authenticating and attaching a DID instance to the Ceramic instance.

### Interact with JSON data

The [Tile document client](../stream-programs/tile-document.md) provides APIs to interact specifically with [Tile document streams](../stream-programs/tile-document.md).

### Interact with crypto account links

The [CAIP-10 links client](../stream-programs/caip10-link.md) provides APIs to interact specifically with [CAIP-10 links streams](../stream-programs/caip10-link.md).
