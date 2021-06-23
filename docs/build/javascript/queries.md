# Queries

This guide demonstrates how to query streams during runtime using the [JS HTTP](./installation.md#js-http-client) and [JS Core](./installation.md#js-core-client) clients.

## **Requirements**
You need to have an [installed client](./installation.md) to perform queries during runtime.

## **Query a stream**
Use the [`loadStream()`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#loadstream){:target="_blank"} method to load a single stream using its *StreamID*.

``` javascript
const streamId = 'kjzl6cwe1jw14...'
const stream = await ceramic.loadStream(streamId)
```

!!! warning "Loading the proper stream type"
    
    When using the Typescript APIs, `loadStream` by default returns an object of type `Stream`, which will not have any methods available to perform updates, or any other streamtype-specific methods or accessors.  To be able to perform updates, as well as to access streamtype-specific data or functionality, you need to specialize the `loadStream` method on the StreamType of the Stream being loaded. For example, to load a `TileDocument`, you would say `await ceramic.loadStream<TileDocument>(streamId)`

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#loadstream){:target="_blank"}

## **Query a stream at a specific commit**

If you want to see the contents of a stream as of a specific point in time, it's possible to pass a *CommitID* instead of a *StreamID* to the `loadStream()` method described above. This will cause the Stream to be loaded at the specified commit, rather than the current commit as loaded from the network. When loading with a CommitID, the returned Stream object will be marked as readonly and cannot be used to perform updates. If you wish to perform updates, load a new instance of the Stream using its StreamID.

## **Query multiple streams**

Use the [`multiQuery()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.ceramicapi-1.html#multiquery){:target="_blank"} method to load multiple streams at once. The returned object is a map from *StreamIDs* to stream instances.

```javascript
const queries = [{
  streamId: 'kjzl6cwe1jw...14'
}, {
  streamId: 'kjzl6cwe1jw...15'
}]
const streamMap = await ceramic.multiQuery(queries)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.multiquery-1.html){:target="_blank"}

## **Query a stream using paths**
Use the [`multiQuery()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.ceramicapi-1.html#multiquery){:target="_blank"} method to load one or more streams using known paths from a root stream to its linked streams.

Imagine a stream `kjzl6cwe1jw...14` whose content contains the StreamIDs of two other streams. These StreamIDs exist at various levels within a nested JSON structure.

```javascript
{
  a: 'kjzl6cwe1jw...15',
  b: {
    c: 'kjzl6cwe1jw...16'
  }
}
```

In the stream above, the path from root stream `kjzl6cwe1jw...14` to linked stream `kjzl6cwe1jw...15` is `/a` and the path to linked stream `kjzl6cwe1jw...16` is `/b/c`. Using the StreamID of the root stream and the paths outlined here, we use `multiQuery()` to query all three streams at once without needing to explicitly know the StreamIDs of the two linked streams.

The `multiQuery()` below will return a map with all three streams.

``` javascript
const queries = [{
  streamId: 'kjzl6cwe1jw...14'
  paths: ['/a', '/b/c']
}]
const streamMap = await ceramic.multiQuery(queries)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.multiquery-1.html){:target="_blank"}


## **Helper methods**
To get specific information about the stream that you created or loaded you can use the accessors on the [`Stream`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html){:target="_blank"} class. Below are some examples.

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html){:target="_blank"}

### Get StreamID
Use the [`stream.id`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html#id){:target="_blank"} property to get the unique [`StreamID`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_streamid.streamid-1.html){:target="_blank"} for this stream.

```javascript
const streamId = stream.id
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html#id){:target="_blank"}

### Get latest commit
Use the [`stream.commitId`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html#commitid){:target="_blank"} property to get latest [CommitID](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_streamid.commitid-1.html) of a stream.

```javascript
const commitId = stream.commitId
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_streamid.commitid-1.html){:target="_blank"}

### Get all anchor commits
Use the [`stream.anchorCommitIds`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html#anchorcommitids){:target="_blank"} property to get all CommitIDs which are anchor commits for this stream.

```javascript
const anchorCommits = stream.anchorCommitIds
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.stream-1.html#anchorcommitids){:target="_blank"}

