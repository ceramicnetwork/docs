# **Caching Ceramic data with Tile Loader**

---

The Glaze Tile Loader library improves application performance by providing batching and caching for tile document streams on the Ceramic network, reducing wait times for retrieving Ceramic data inside your application.

## **How it works**

---

Tile loader provides a thin abstraction on top of the [`TileDocument`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html) stream client to add the following performance improvements:

- **Batching** – groups concurrent loading of multiple streams in a single call
- **Caching** – stores already loaded streams client-side to avoid making repeated requests to the network

The [`TileLoader` class](../../reference/glaze/classes/tile_loader.TileLoader.md) provided by the library extends the `DataLoader` class from the [dataloader library](https://github.com/graphql/dataloader) to leverage its capabilities, and provides [extra methods](#additional-apis) to support additional use cases for Ceramic.

## **Getting started with Tile Loader**

---

Visit the Glaze [**Tile Loader reference →**](../../reference/glaze/modules/tile_loader.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with tile loader:

### **Batching**

Batching combines multiple concurrent queries to a Ceramic node into a single query.

Without tile loader, a Ceramic client will retrieve two tile document streams from the network by performing two separate network requests:

```ts
import { TileDocument } from '@ceramicnetwork/stream-tile'

const [stream1, stream2] = await Promise.all([
  TileDocument.load(ceramic, 'streamID1'),
  TileDocument.load(ceramic, 'streamID2'),
])
```

When using the `loader` instance, the two streams will now be loaded with a single network request:

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic })
const [stream1, stream2] = await Promise.all([
  loader.load('streamID1'),
  loader.load('streamID2'),
])
```

### **Caching**

Caching allows your application to keep track of streams client-side that have been previously retrieved from the network.

!!! warning ""

    :octicons-stop-16: Caching is **disabled by default** and **may not be suited for your use-cases**, make sure you carefully consider the trade-offs before enabling it.

    Streams loaded from cache may be out of sync with the current state found on the network, so applications should be designed accordingly!

Without tile loader, the Ceramic client will perform two network requests to get the state of the stream at two different times:

```ts
import { TileDocument } from '@ceramicnetwork/stream-tile'

// Load the stream at some point in your app
const stream = await TileDocument.load(ceramic, 'streamID')
// Maybe the same stream needs to be loaded at a different time or in another part of your app
const streamAgain = await TileDocument.load(ceramic, 'streamID')
```

When using the `loader` instance, the second call will be resolved _directly from the loader's internal cache_ rather than making a second network request:

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic, cache: true })

// Load the stream at some point in your app
const stream = await loader.load('streamID')
// Maybe the same stream needs to be loaded at a different time or in another part of your app
const streamAgain = await loader.load('streamID')
```

#### **Customizing your cache**

When setting the `cache` option to `true` in the loader constructor, the cache will live as long as the loader instance. This means that any stream will only ever get loaded from the network once, and persist in memory until the loader instance is deleted.

It is possible to provide a custom cache implementation in the loader constructor to customize this behavior, for example in order to limit memory usage by restricting the number of streams kept in the cache, or discarding loaded streams after a given period of time.

A custom cache must implement a subset of the Map interface, defined by the [TileCache](../../reference/glaze/modules/tile_loader.md#tilecache) interface.

```ts
import { TileLoader } from '@glazed/tile-loader'

// The cache must implement a subset of the Map interface
const cache = new Map()
const loader = new TileLoader({ ceramic, cache })

// The loader will cache the request as soon as the load() method is called, so the stored value is a Promise of a TileDocument
loader.load('streamID')
cache.get('streamID') // Promise<TileDocument>
```

## **Additional APIs**

---

In addition to the [`DataLoader APIs`](https://github.com/graphql/dataloader#api), the [`TileLoader` class](../../reference/glaze/classes/tile_loader.TileLoader.md) provides the following methods specific to Ceramic's use-cases.

### **Create tile documents**

The [`create()` method](../..//reference/glaze/classes/tile_loader.TileLoader.md#create) wraps a [`TileDocument.create()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create) call to add the created stream to the internal cache of the loader. This has no effect if the cache is disabled.

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic, cache: true })
const stream = await loader.create({ hello: world })
// The following call will returne the stream from the cache
await loader.load(stream.id)
```

### **Retrieve deterministic tiles**

In Ceramic, streams can be loaded using their metadata rather than a stream ID if they are created as determinitic streams.

Using the [`deterministic()` method](../..//reference/glaze/classes/tile_loader.TileLoader.md#deterministic) of a loader instance allows to load such streams while benefiting from the batching and caching functionalities of the loader.

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic })
// The following call will load the latest version of the stream based on its metadata, if such stream exists
const stream = await loader.deterministic({
  controllers: ['did:key:...'],
  family: 'test',
})
```
