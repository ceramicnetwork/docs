# TileDocument API

This guide describes how to create, update, and query [TileDocuments](./overview.md) using the [JS HTTP Client](../../build/javascript/installation.md#js-http-client) and the [Core Client](../../build/javascript/installation.md#js-core-client). You can also interact with TileDocuments from the [CLI](../../build/cli/installation.md); see the [Quick Start](../../build/cli/quick-start.md) guide for more information.

## **Requirements**

You need an [installed client](../../build/javascript/installation.md) and an [authenticated user](../../build/javascript/authentication.md) to perform writes to TileDocuments. If you only wish to query TileDocuments then you do not need authentication.

## **Installation**

`npm install @ceramicnetwork/stream-tile`

## **Write APIs**

### **Create a TileDocument**

Use the [`TileDocument.create()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="\_blank"} method to create a new TileDocument.

```javascript
import { TileDocument } from '@ceramicnetwork/stream-tile'

const doc = await TileDocument.create(ceramic, content, metadata, opts)

const streamId = doc.id.toString()
```

In this example we create a TileDocument where we set `content`, `schema`, `controllers`, and `family`.

```javascript
const doc = await TileDocument.create(
  ceramic,
  { foo: 'bar' },
  {
    controllers: [ceramic.did.id],
    family: 'doc family',
    schema: schemaDoc.commitId,
  },
)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="\_blank"}

#### Parameters

##### ceramic

When creating a TileDocument, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic-1.html){:target="\_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="\_blank"} when using the HTTP client.

##### content

The content of your TileDocument, in the form of a JSON document. If `schema` is set in the metadata, then the content must conform to the specified schema.

!!! warning ""

    When `content` is included during document creation, the document's *genesis commit* will be signed by the authenticated user's DID. When `content` is omitted (set as `null` or `undefined`), then the *genesis commit* will not be signed.

##### metadata (optional)

| Parameter       | Required? | Value            | Description                                                                                                     | Notes                                                       |
| --------------- | --------- | ---------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- |
| `controllers`   | optional  | array of strings | Defines the DID that is allowed to modify the document. Currently only one controller is supported per document | If empty, defaults to currently authenticated DID           |
| `schema`        | optional  | string           | CommitID of a Ceramic TileDocument that contains a JSON schema                                                  | If set, schema will be enforced on content                  |
| `family`        | optional  | string           | Allows you to group similar documents into _families_                                                           | Useful for indexing groups of like documents on the network |
| `tags`          | optional  | array of strings | Allows you to tag similar documents                                                                             | Useful for indexing groups of like documents on the network |
| `deterministic` | optional  | boolean          | If false, allows TileDocuments with the same `content` and `metadata` to generate unique StreamIDs              | If empty, defaults to false                                 |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html){:target="\_blank"}

!!! warning "Using the `deterministic` parameter"

    For most use cases you will likely want to leave the `deterministic` parameter set to false. However for special circumstances, you may want this to be set to true. For example this should be set to true if you would like to enable [deterministic queries](#query-a-deterministic-tiledocument) for your TileDocument. If this is your use case, then it is also important that you set `content` to `null` during document creation. Deterministic document queries are based entirely on the document's initial metadata. You can proceed to add content to your document by [updating it](#update-a-tiledocument).

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html#deterministic){:target="\_blank"}

##### opts (optional)

The final argument to `TileDocument.create` is an instance of [`CreateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="\_blank"}, which are options that control network behaviors performed as part of the operation. They are not included in the document itself.

| Parameter            | Required? | Value   | Description                                                                       | Default value                              |
| -------------------- | --------- | ------- | --------------------------------------------------------------------------------- | ------------------------------------------ |
| `anchor`             | optional  | boolean | Request an anchor after creating the document                                     | true                                       |
| `publish`            | optional  | boolean | Publish the new document to the network                                           | true                                       |
| `sync`               | optional  | enum    | Controls behavior related to syncing the current document state from the network  | PREFER_CACHE                               |
| `syncTimeoutSeconds` | optional  | number  | How long to wait to hear about the current state of the document from the network | 3 for deterministic documents, 0 otherwise |
| `pin`                | optional  | boolean | Whether to immediately pin the stream upon creation on the connected node         | false                                      |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="\_blank"}

### **Update a TileDocument**

Use the [`doc.update()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#update){:target="\_blank"} method to update the `content` or `metadata` of an existing TileDocument.

```javascript
const doc = await TileDocument.load(ceramic, streamId, opts)
await doc.update(newContent, newMetadata, opts)
```

In this example we update a TileDocument's content while also giving it a tag.

```javascript
const streamId = 'kjzl6cwe1jw14...' // Replace this with the StreamID of the TileDocument to be updated
const doc = await TileDocument.load(ceramic, streamId)
await doc.update({ foo: 'baz' }, { tags: ['baz'] })
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#update){:target="\_blank"}

!!! warning "Persisting updates"

    Please note that if you want updates to a Stream to persist you need to ensure that the stream is pinned by at least one node on the network. See the [pinning](../../build/javascript/pinning.md) page for more information.


#### Parameters

##### content

The new content of your document. This fully replaces any existing content in the document.

##### metadata (optional)

Only fields that are provided in the metadata arg will be updated. Fields not specified
in the metadata arg will be left with their current value.

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html){:target="\_blank"}

!!! warning "Updating the `deterministic` parameter"

    Please note that the `deterministic` parameter can only be set while creating a document. It cannot be updated once the document exists.

##### opts (optional)

The final argument to `TileDocument.update` is an instance of [`UpdateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="\_blank"}, which are options that control network behaviors performed as part of the operation. They are not included in the document itself.

| Parameter | Required? | Value   | Description                                   | Default value |
| --------- | --------- | ------- | --------------------------------------------- | ------------- |
| `anchor`  | optional  | boolean | Request an anchor after updating the document | true          |
| `publish` | optional  | boolean | Publish the update to the network             | true          |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="\_blank"}

## **Read APIs**

### **Load a TileDocument**

Use the [`TileDocument.load()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#load){:target="\_blank"} method to load a single document using its _StreamID_

```javascript
const streamId = 'kjzl6cwe1jw14...' // Replace this with the StreamID of the TileDocument to be loaded
const doc = await TileDocument.load(ceramic, streamId, opts)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#load){:target="\_blank"}

#### Parameters

##### ceramic

When loading a TileDocument, the first parameter is the `CeramicAPI` used to communicate with the Ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic-1.html){:target="\_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="\_blank"} when using the HTTP client.

##### streamId

The StreamID or CommitID of the TileDocument to be loaded. When providing the document's StreamID, Ceramic will attempt to load the most recent version of the document from the network. If a CommitID is provided instead, Ceramic will load the document with the version of the contents and metadata from the specific commit specified. The returned TileDocument object will also be marked readonly and cannot be used to perform updates.

##### opts (optional)

The final argument to `TileDocument.load` is an instance of [`LoadOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.loadopts-1.html){:target="\_blank"}, which are options that control network behaviors performed as part of the operation.

| Parameter            | Required? | Value  | Description                                                                       | Default value            |
| -------------------- | --------- | ------ | --------------------------------------------------------------------------------- | ------------------------ |
| `sync`               | optional  | enum   | Controls behavior related to syncing the current document state from the network  | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional  | number | How long to wait to hear about the current state of the document from the network | 3                        |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.loadopts-1.html){:target="\_blank"}

### **Query a deterministic TileDocument**

In the [create](#create-a-tiledocument) section, we discussed how to create a document with the `deterministic` parameter set to true. Since this parameter allows us to generate the exact same StreamID if we create two documents with the same initial `metadata`, it is possible to "query" the document using the same [`TileDocument.create`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="\_blank"} method that we used to initially create it, without needing to know the StreamID before performing the query. Note we are setting the `CreateOpts` parameters (`anchor` and `publish`) to false so that we are only loading the document and not publishing any changes to the network.

```javascript
const doc = TileDocument.create(
  ceramic,
  null,
  {
    controllers: ['did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H'],
    family: 'example family',
    deterministic: true,
  },
  { anchor: false, publish: false }
})
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="\_blank"}
