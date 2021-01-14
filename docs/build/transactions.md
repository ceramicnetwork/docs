# Transactions
Transactions are interactions that write to Ceramic, such as creating new documents or modifying existing documents. This guide demonstrates how to make transactions during runtime using the [HTTP](../reference/javascript/clients.md#http-client) and [core](../reference/javascript/clients.md#core-client) clients. To make transactions using the CLI, see [Quick Start](quick-start.md).

## Prerequisites
You need an [installed client](installation.md) and an [authenticated user](authentication.md) to perform transactions on the network during runtime.

## Create a document
Use the [`createDocument()`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#createdocument){:target="_blank"} method to create a new document.

```javascript
const doc = await ceramic.createDocument(doctype, docParams, docOpts)
```

**Example**

In this example we create a document where we set `doctype`, `content`, `schema`, `controllers`, and `family`.

```javascript
const doc = await ceramic.createDocument('tile', {
  content: { foo: "bar" }
  metadata: {
    schema: "ceramic://kyz123...456",
    controllers: ["did:3:kyz123...456"],
    family: "doc family"
  }
})
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#createdocument){:target="_blank"}

### Parameters
When creating a document, the first parameter is the `doctype` and it is always required. Other parameters including `content` will vary depending on the *doctype* specification. If you do not specify a *doctype* or if your parameters do not meet the doctype's requirements, your transaction will fail.

#### Doctype
Doctypes are rules that govern the behavior of documents on the Ceramic network. A doctype is required.

| Parameter     | Required?   | Value            | Description |
| ------------- | ----------- | ---------------- | ----------- |
| `doctype`     | required    | string           | Specifies rules for `content`, `metadata`, and conflict resolution |

!!! note "Supported doctypes"
    Ceramic currently supports two doctypes: `tile` for storing arbitrary JSON content and `caip-10 link` for storing a proof that binds a DID to a blockchain account. Read each doctype's documentation for more information on its parameters below.

#### DocParams
DocParams are specifics related to your document including `content`, `metadata`, and `deterministic`. All of these properties are optional.

##### Content

| Parameter     | Required?   | Value            | Description | Notes |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `content`     | optional    | object           | The content of your document | Must conform to the `doctype` and `schema` if present |

!!! note ""
    When `content` is included during document creation, the document's *genesis commit* will be signed by the authenticated user's DID. When `content` is omitted, then the *genesis commit* will not be signed.

##### Metadata

| Parameter     | Required?   | Value               | Description | Notes |
| ------------- | ----------- | ------------------- | ----------- | ----- |
| `schema`      | optional    | string              | URL of a Ceramic document that contains a JSON schema  | If set, schema will be enforced on content |
| `controllers` | optional    | array of strings    | Defines the DID that is allowed to modify the document | If empty, defaults to currently authenticated DID |
| `family`      | optional    | string              | Allows you to group similar documents into *families* | Useful for indexing groups of like documents on the network | 

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docmetadata-1.html){:target="_blank"}

##### Deterministic

| Parameter         | Required?   | Value            | Description | Notes |
| ----------------- | ----------- | ---------------- | ----------- | ----- |
| `deterministic`   | optional    | boolean          | If false, allows documents with the same `doctype`, `content`, and `metadata` to generate unique DocIDs | If empty, defaults to false |

!!! note "Using the `deterministic` parameter"
    For most use cases you will likely want to leave the `deterministic` parameter set to false. However for special circumstances, you may want this to be set to true. For example this should be set to true if you would like to enable [deterministic queries](queries.md#query-a-deterministic-document) for your document. If this is your use case, then it is also important that you omit all `content` during document creation. You can proceed to add content to your document by [updating it](#update-a-document).

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docparams-1.html#deterministic){:target="_blank"}

#### DocOpts (optional)
DocOpts are options that can be passed to each operation on a document (create, change, load) that control network behaviors performed as part of the operation.  They are not included in the document itself.

| Parameter     | Required?   | Value            | Description | Default value |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          | Request an anchor after the update was made | true |
| `publish`     | optional    | boolean          | Publish the update to the network | true |
| `sync`        | optional    | boolean          | Sync document updates from the network | true |
 
[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docopts-1.html){:target="_blank"}


## Update a document
Use the [`doc.change()`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.doctype-1.html#change){:target="_blank"} method to update the `content` or `metadata` of your document. Any update to a document must conform to the update rules specified by the document's `doctype`. Note that you can also pass `DocOpts` parameters to this method if you wish to not use the default behavior of publishing and/or anchoring your change, but this behavior should be reserved for advanced use cases.

```javascript
await doc.change({ content: { foo: "updated" }})
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.doctype-1.html#change){:target="_blank"}

## Utilities

### Get DocID of a document
Use the [`doc.id`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.doctype-1.html#id){:target="_blank"} property to get the unique [`DocID`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_docid.docid.html){:target="_blank"} for this document.

```javascript
const docId = doc.id
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.doctype-1.html#id){:target="_blank"}

### Check anchor status (OED FIX)
Use the [`doc.id`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.doctype-1.html#id){:target="_blank"} property to get the unique [`DocID`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_docid.docid.html){:target="_blank"} for this document.

```javascript
const docId = doc.id
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_common.doctype-1.html#id){:target="_blank"}


</br>
</br>
</br>
