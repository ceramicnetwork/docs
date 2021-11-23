# TileDocument.js

TileDocument.js is a library used to interact. TileDocument is a StreamType that stores a mutable JSON document, providing similar functionality as a NoSQL document store.

##### Things to Know

- TileDocuments are streams used for storing JSON documents. The TileDocument stream is structured as a single log of [commits](../../learn/glossary.md#commits), where each commit only contains the diff from the previous version. Optionally, TileDocuments may specify a JSON schema and all commits must adhere to the schema.
- TileDocuments rely on [anchor commits](../../learn/glossary.md#anchor-commit) for providing immutable timestamps for the [genesis commit](../../learn/glossary.md#genesis-commit) and subsequent [signed commits](../../learn/glossary.md#signed-commit) in the stream. In the case of conflicting versions, the branch with the earliest recorded anchor commit will be respected as the canonical branch.
- TileDocuments rely on [DIDs](../../learn/glossary.md#dids) for [authentication](../../learn/glossary.md#authentication). Only the DID(s) assigned as the [controller](../../learn/glossary.md#controllers) of the stream are allowed to perform writes.
- As more updates are made to a single tile, the underlying DAG grows linearly, and so does sync times when fetching the stream over the network. When loading a tile from a node that already has it present, reqponses are very quick
- This guide describes how to create, update, and query [TileDocuments](./overview.md) using the [JS HTTP Client](../../build/javascript/installation.md#js-http-client) and the [Core Client](../../build/javascript/installation.md#js-core-client). You can also interact with TileDocuments from the [CLI](../../build/cli/installation.md); see the [Quick Start](../../build/cli/quick-start.md) guide for more information.

## Installation

---

> **REQUIREMENTS** 
>
>You need an [installed client](../../build/javascript/installation.md) and an [authenticated user](../../build/javascript/authentication.md) to perform writes to TileDocuments. If you only wish to query TileDocuments then you do not need authentication.

First, install TileDocument.js from npm:

```ts
npm install @ceramicnetwork/stream-tile
```

Then, include TileDocument.js in your project:

```ts

```

## Explore the API

---

### [Create a Tile →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="\_blank"}

Use the `TileDocument.create()` method to create a new TileDocument.

```ts
await TileDocument.create(ceramic, content, metadata, opts)
```

##### Parameters

`ceramic`: When creating a TileDocument, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic-1.html){:target="\_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="\_blank"} when using the HTTP client.

`content`: The content of your TileDocument, in the form of a JSON document. If `schema` is set in the metadata, then the content must conform to the specified schema. When `content` is included during document creation, the document's *genesis commit* will be signed by the authenticated user's DID. When `content` is omitted (set as `null` or `undefined`), then the *genesis commit* will not be signed.

[`metadata`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html){:target="\_blank"} – An object that specifies various metadata values for the Tile. *Optional*

| Parameter                        | Required? | Value            | Description                                                                                                                                                                                                    | Notes                                                       |
| -------------------------------- | --------- | ---------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `controllers`                    | optional  | array of strings | Defines the DID that is allowed to modify the document. Currently only one controller is supported per document                                                                                                | If empty, defaults to currently authenticated DID           |
| `schema`                         | optional  | string           | CommitID of a Ceramic TileDocument that contains a JSON schema                                                                                                                                                 | If set, schema will be enforced on content                  |
| `family`                         | optional  | string           | Allows you to group similar documents into _families_                                                                                                                                                          | Useful for indexing groups of like documents on the network |
| `tags`                           | optional  | array of strings | Allows you to tag similar documents                                                                                                                                                                            | Useful for indexing groups of like documents on the network |

[`createOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="\_blank"} – Instructions for what actions a Ceramic node should take after receiving the transaction from your client. Note, these options are not included in the tile itself. *Optional*

| Parameter            | Required? | Value   | Description                                                                       | Default value                              |
| -------------------- | --------- | ------- | --------------------------------------------------------------------------------- | ------------------------------------------ |
| `anchor`             | optional  | boolean | Request an anchor after creating the tile                                     | true                                       |
| `publish`            | optional  | boolean | Publish the new tile to the network                                           | true                                       |
| `sync`               | optional  | enum    | Controls behavior related to syncing the tile's current state from the network  | PREFER_CACHE                               |
| `syncTimeoutSeconds` | optional  | number  | How long to wait to hear about the current state of the document from the network | 0 |
| `pin`                | optional  | boolean | Immediately pin the tile upon creation on the Ceramic node         | false                                      |

---

### [Create a Tile Deterministically →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#deterministic){:target="\_blank"}

Use the `TileDocument.deterministic()` method to create a new deterministic TileDocument or to query an existing deterministic TileDocument (see the [Query A Deterministic TileDocument](#query-a-deterministic-tiledocument) section).

```ts
await TileDocument.deterministic(ceramic, metadata, opts)
```

!!! warning "Using the `deterministic` function"

    For most use cases you will likely want to use [`TileDocument.create()`](#create-a-tiledocument). However for special circumstances, you may want to use [`TileDocument.deterministic()`](#create-a-deterministic-tiledocument) . For example you should use [`TileDocument.deterministic()`](#create-a-deterministic-tiledocument)  if you would like to enable [deterministic queries](#query-a-deterministic-tiledocument) for your TileDocument. If this is your use case, then it is also important that you provide initial metadata as deterministic document queries are based entirely on the document's initial metadata. You can proceed to add content to your document by [updating it](#update-a-tiledocument).

##### Parameters

[`ceramic`]() – same as ceramic parameter in `TileDocument.create`.

[`metadata`]() – Metadata is required when creating TileDocuments using [`TileDocument.deterministic`](#create-a-deterministic-tiledocument). Specifically the `family` and/or `tag` parameters are required. For more information see the [`metadata`](#metadata-optional) parameter in [`TileDocument.create`](#create-a-tiledocument).

[`createOpts`]() – same as [`opts`](#opts-optional) parameter in [`TileDocument.create`](#create-a-tiledocument).

---

### [Update a Tile →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#update){:target="\_blank"}

Use the `doc.update()`method to update the `content` or `metadata` of an existing tile. Note, it's best practice to load the tile prior to performing updates. This ensures you're operating on the most current version of the tile.

``` ts
const doc = await TileDocument.load(ceramic, streamId, opts);
await doc.update(newContent, newMetadata, opts);
```

!!! warning "Persisting updates"

    Please note that if you want updates to a Stream to persist you need to ensure that the stream is pinned by at least one node on the network. See the [pinning](../../build/javascript/pinning.md) page for more information.

##### Parameters

[`content`]() – the new content of your tile. This operation *fully replaces* any existing content in the tile.

[`metadata`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html){:target="\_blank"} – Only specified fields will be updated; fields not specified are unaffected.

[`updateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="\_blank"} – Instructions for what actions a Ceramic node should take after receiving the transaction from your client. Note, these options are not included in the tile itself. *Optional*

| Parameter | Required? | Value   | Description                                   | Default value |
| --------- | --------- | ------- | --------------------------------------------- | ------------- |
| `anchor`  | optional  | boolean | Request an anchor after updating the document | true          |
| `publish` | optional  | boolean | Publish the update to the network             | true          |

---

### [Load a Tile →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#load){:target="\_blank"}

Use the [`TileDocument.load()`] method to load a single document using its _StreamID_.

``` ts
const streamId = 'kjzl6cwe1jw14...'; // Replace with StreamID of the Tile to be loaded
const doc = await TileDocument.load(ceramic, streamId, opts);
```

##### Parameters

[`ceramic`]() – When loading a TileDocument, the first parameter is the `CeramicAPI` used to communicate with the Ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic-1.html){:target="\_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="\_blank"} when using the HTTP client.

[`streamID`]() - The StreamID or CommitID of the Tile to be loaded. When providing the document's StreamID, Ceramic will attempt to load the most recent version of the document from the network. If a CommitID is provided instead, Ceramic will load the document with the version of the contents and metadata from the specific commit specified. The returned TileDocument object will also be marked readonly and cannot be used to perform updates.

[`loadOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.loadopts-1.html){:target="\_blank"} – 
Options that control network behaviors performed as part of the operation.

| Parameter            | Required? | Value  | Description                                                                       | Default value            |
| -------------------- | --------- | ------ | --------------------------------------------------------------------------------- | ------------------------ |
| `sync`               | optional  | enum   | Controls behavior related to syncing the current document state from the network  | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional  | number | How long to wait to hear about the current state of the document from the network | 3                        |

---

### [Query a deterministic Tile →](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#deterministic){:target="\_blank"}

In the [deterministic](#create-a-deterministic-tiledocument) section, we discussed how to create a deterministic document. Since this function allows us to generate the exact same StreamID if we use it to create two documents with the same initial `metadata`, it is possible to "query" the document using the same [`TileDocument.deterministic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#deterministic){:target="\_blank"} method that we used to initially create it, without needing to know the StreamID before performing the query. Note we are setting the `CreateOpts` parameters (`anchor` and `publish`) to false so that we are only loading the document and not publishing any changes to the network. See the [example below]().


> See [TileDocument API](./api.md) for instructions on how to create and update TileDocument streams. You can query TileDocuments using Ceramic's standard [queries API](../../build/javascript/queries.md).


## Examples

---

### Create a Tile that stores a JSON schema
In this example we create a TileDocument where we set `content`, `schema`, `controllers`, and `family`.

``` ts
const doc = await TileDocument.create(
  ceramic,
  { foo: 'bar' },
  {
    controllers: [ceramic.did.id],
    family: 'doc family',
    schema: schemaDoc.commitId,
  }
);
```

### Create a Tile that uses a JSON schema

``` ts
.
```

### Create and query a deterministic tile

In this example we create a deterministic TileDocument where we set `tags`, and `family` in the metadata. We then can retrieve that same tile document using `TileDocument.deterministic` as long as we use the same metadata.

``` ts
// create a new Tile
const doc = await TileDocument.create(ceramic, {
  family: 'doc family',
  tags: ['tag1'],
});

// load a Tile deterministically
// all content and metadata need to be exactly the same
const retrievedDoc = await TileDocument.deterministic(ceramic,
  { family: 'doc family', tags: ['tag1'] },
  { anchor: false, publish: false }
);

// proof that the two responses are equal
console.log(doc.id.toString() === retrievedDoc.id.toString());

// true
```

### Update a Tile

Here we update a Tile's content while also adding a tag to its metadata.

``` ts
// streamID of the Tile to update
const streamId = 'kjzl6cwe1jw14...';

// load Tile from the network
const doc = await TileDocument.load(ceramic, streamId);

// perform update
await doc.update({ foo: 'baz' }, { tags: ['baz'] });
```

### Load a Tile

Here we load the current state of a Tile from the network using its StreamID.

``` ts
// streamID of the tile you want to load
const streamId = 'kjzl6cwe1jw14...'; 

// load the current state from the network
const doc = await TileDocument.load(ceramic, streamId, opts);
```

## Additional Resources

---

- [CIP-8: Tile Document Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md)
- [Complete TileDocument.js API Reference]()

## Next Steps

---

- [Add decentralized indexing so you don't need to keep track of streamIDs between sessions]()