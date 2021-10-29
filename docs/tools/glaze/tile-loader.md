# Tile Loader

The Tile Loader library provides a thin abstraction over the [`TileDocument`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html) stream client to perform **batching** and **caching** of streams in order to improve performance.

The [`TileLoader` class](../../reference/glaze/classes/tile_loader.TileLoader.md) extends the [`DataLoader` class from the dataloader library](https://github.com/graphql/dataloader) to leverage its [batching](#batching) and [caching](#caching) capacities, as well as adding [extra methods](#additional-apis) to support common use-cases.

## Batching

Batching consists in the process of combining multiple concurrent queries to a Ceramic node into a single one.

In the following example, the Ceramic client will perform **two separate requests** to the Ceramic node:

```ts
import { TileDocument } from '@ceramicnetwork/stream-tile'

const [stream1, stream2] = await Promise.all([
  TileDocument.load(ceramic, 'streamID1'),
  TileDocument.load(ceramic, 'streamID2'),
])
```

Using a loader instance instead as in the following example, the two streams will be loaded with **a single request** to the Ceramic node:

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic })
const [stream1, stream2] = await Promise.all([
  loader.load('streamID1'),
  loader.load('streamID2'),
])
```

## Caching

Caching consists in keeping track of streams loaded from a Ceramic node.

!!! warning ""

    :octicons-stop-16: Caching is **disabled by default** and **may not be suited for your use-cases**, make sure you carefully consider the trade-offs before enabling it.

    Streams loaded from the cache may be out of date from the state on the Ceramic network, so applications should be designed accordingly.

In the following example, the Ceramic client will perform **two requests** to the Ceramic node:

```ts
import { TileDocument } from '@ceramicnetwork/stream-tile'

// Load the stream at some point in your app
const stream = await TileDocument.load(ceramic, 'streamID')
// Maybe the same stream needs to be loaded at a different time or in another part of your app
const streamAgain = await TileDocument.load(ceramic, 'streamID')
```

Using a loader instance instead as in the following example, the second call will be resolved **directly from the loader's internal cache** rather than making another request to the Ceramic node:

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic, cache: true })

// Load the stream at some point in your app
const stream = await loader.load('streamID')
// Maybe the same stream needs to be loaded at a different time or in another part of your app
const streamAgain = await loader.load('streamID')
```

### Custom cache

When setting the `cache` option to `true` in the loader constructor, the cache will live as long as the loader instance. This means any individual stream will only ever get loaded once, and persist in memory until the loader instance is deleted.

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

## Additional APIs

In addition to the [`DataLoader APIs`](https://github.com/graphql/dataloader#api), the [`TileLoader` class](../../reference/glaze/classes/tile_loader.TileLoader.md) provides the following methods specific to Ceramic's use-cases.

### Tile creation

The [`create()` method](../..//reference/glaze/classes/tile_loader.TileLoader.md#create) wraps a [`TileDocument.create()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create) call to add the created stream to the internal cache of the loader. This has no effect if the cache is disabled.

```ts
import { TileLoader } from '@glazed/tile-loader'

const loader = new TileLoader({ ceramic, cache: true })
const stream = await loader.create({ hello: world })
// The following call will returne the stream from the cache
await loader.load(stream.id)
```

### Deterministic tiles loading

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
