# TileDocument API

This guide demonstrates how to create, update, and query TileDocuments on the Ceramic network using the [HTTP](../reference/javascript/clients.md) and [core](../reference/javascript/clients.md) clients.

## Prerequisites
You need an [installed client](installation.md) and an [authenticated user](authentication.md) to perform writes to TileDocuments on the network during runtime. If you only wish to query existing TileDocuments then you still need an installed client but it doesn't need to be authenticated.

## Create a document
Use the [`TileDocument.create()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="_blank"} method to create a new document.

```javascript
const doc = await TileDocument.create(ceramic, content, metadata, opts)
```

**Example**

In this example we create a document where we set `content`, `schema`, `controllers`, and `family`.

```javascript
const doc = await TileDocument.create(ceramic,
  { foo: "bar" },
  {
    controllers: [ceramic.did.id],
    family: "doc family",
    schema: schemaDoc.commitId,
  }
)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="_blank"}


### Parameters

#### ceramic


When creating a TileDocument, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"} when using the HTTP client.

#### content

The content of your document, in the form of a JSON document. If `schema` is set in the metadata, then the content must conform to the specified schema.

!!! info ""
    When `content` is included during document creation, the document's *genesis commit* will be signed by the authenticated user's DID. When `content` is omitted (set as `null` or `undefined`), then the *genesis commit* will not be signed.

#### metadata (optional)

| Parameter       | Required?   | Value               | Description | Notes |
| --------------- | ----------- | ------------------- | ----------- | ----- |
| `schema`        | optional    | string              | CommitID of a Ceramic TileDocument that contains a JSON schema  | If set, schema will be enforced on content |
| `controllers`   | optional    | array of strings    | Defines the DID that is allowed to modify the document. Currently only one controller is supported per document | If empty, defaults to currently authenticated DID |
| `family`        | optional    | string              | Allows you to group similar documents into *families* | Useful for indexing groups of like documents on the network |
| `tags`        | optional    | array of strings              | Allows you to tag similar documents | Useful for indexing groups of like documents on the network |
| `deterministic` | optional    | boolean          | If false, allows TileDocuments with the same `content` and `metadata` to generate unique StreamIDs | If empty, defaults to false |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html){:target="_blank"}


!!! info "Using the `deterministic` parameter"
    For most use cases you will likely want to leave the `deterministic` parameter set to false. However for special circumstances, you may want this to be set to true. For example this should be set to true if you would like to enable [deterministic queries](../../../build/queries/#query-a-deterministic-document) for your document. If this is your use case, then it is also important that you think very carefully about what `content` is provided when first creating the document, as that content is what will need to be provided again in order to deterministically query the document. It is recommended that the document be created with the minimal possible content to uniquely identify the document. You can proceed to add additional content to your document by [updating it](#update-a-document).

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html#deterministic){:target="_blank"}


#### opts (optional)
The final argument to `TileDocument.create` is an instance of [`CreateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="_blank"}), which are options that control network behaviors performed as part of the operation.  They are not included in the document itself.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after creating the document | true |
| `publish`     | optional    | boolean          | Publish the new document to the network | true |
| `sync`        | optional    | enum             | Controls behavior related to syncing the current document state from the network | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional    | number            | How long to wait to hear about the current state of the document from the network | 3 for deterministic documents, 0 otherwise |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="_blank"}


## Update a document
Use the [`doc.update()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#update){:target="_blank"} method to update the `content` or `metadata` of an existing TileDocument.

```javascript
const doc = await TileDocument.create(ceramic, content, metadata, opts)
await doc.update(newContent, newMetadata, opts)
```

**Example**

In this example we update a document's content while also giving it a tag.

```javascript
const doc = await TileDocument.create(ceramic, { foo: "bar" })
await doc.update({ foo: 'baz'}, { tags: ['baz'] })
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#update){:target="_blank"}


### Parameters

#### content

The new content of your document. This fully replaces any existing content in the document.

#### metadata (optional)

Only fields that are provided in the metadata arg will be updated.  Fields not specified
in the metadata arg will be left with their current value within the document.

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_stream_tile.tilemetadataargs-1.html){:target="_blank"}


!!! info "Updating the `deterministic` parameter"
Please note that the `deterministic` parameter can only be set while creating a document. It cannot be updated once the document exists.


#### opts (optional)
The final argument to `TileDocument.update` is an instance of [`UpdateOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="_blank"}), which are options that control network behaviors performed as part of the operation.  They are not included in the document itself.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after updating the document | true |
| `publish`     | optional    | boolean          | Publish the update to the network | true |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="_blank"}

## Load a document
Use the [`TileDocument.load()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#load){:target="_blank"} method to load a single document using its *StreamID*

```javascript
const streamId = 'kjzl6cwe1jw14...'
const doc = await TileDocument.load(ceramic, streamId, opts)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#load){:target="_blank"}


### Parameters

#### ceramic

When loading a TileDocument, the first parameter is the `CeramicAPI` used to communicate with the ceramic node and it is always required. It will either be an instance of [`Ceramic`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"} when using the Core client or an instance of [`CeramicClient`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"} when using the HTTP client.


#### streamId

The StreamID or CommitID of the TileDocument to be loaded. When providing the document's StreamID, Ceramic will attempt to load the most recent version of the document from the network. If a CommitID is provided instead, Ceramic will load the document with the version of the contents and metadata from the specific commit specified. The returned TileDocument object will also be marked readonly and cannot be used to perform updates.


#### opts (optional)
The final argument to `TileDocument.load` is an instance of [`LoadOpts`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.loadopts-1.html){:target="_blank"}), which are options that control network behaviors performed as part of the operation.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `sync`        | optional    | enum             | Controls behavior related to syncing the current document state from the network | SyncOptions.PREFER_CACHE |
| `syncTimeoutSeconds` | optional    | number            | How long to wait to hear about the current state of the document from the network | 3 |

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.loadopts-1.html){:target="_blank"}

## Load a deterministic document
In the [create](api.md#create-a-document) section, we discussed how to create a document with the `deterministic` parameter set to true. Since this parameter allows us to generate the exact same StreamID if we create two documents with the same `content` and `metadata`, it is possible to "query" the document using the same [`TileDocument.create`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html#create){:target="_blank"} method that we used to initially create it, without needing to know the StreamID before performing the query. Note we are setting the `CreateOpts` parameters (`anchor` and `publish`) to false so that we are only loading the document and not publishing any changes to the network.

``` javascript
const doc = TileDocument.create(ceramic,
  { data: 'this data along with the controller and family uniquely identify this document!' },
  {
    controllers: ['did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H'],
    family: 'example family',
    deterministic: true,
  },
  { anchor: false, publish: false }
})
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#createdocument){:target="_blank"}




</br>
</br>
</br>
