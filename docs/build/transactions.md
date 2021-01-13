# Transactions
Transactions are interactions that write to Ceramic, such as creating new documents or modifying existing documents. This guide demonstrates how to make transactions during runtime using the [HTTP]() and [core]() clients. To make transactions using the CLI, see [Quick Start](quick-start.md).

## Prerequisites
You need an [installed client](installation.md) and an [authenticated user](authentication.md) to perform transactions on the network during runtime.

## Create a document
Use the [`createDocument()`](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#createdocument) method to create a new document.

```javascript
const doc = await ceramic.createDocument('doctype', { 
  content: {
    foo: "bar"
  }, 
  metadata: {
    schema: "ceramic://kyz123...456",
    controllers: ["did:3:kyz123...456"],
    family: "doc family"
  }, 
  deterministic: false,
}, 
{
  anchor: "",
  publish: "",
  sync: ""
})
```

Example:

```javascript
const doc = await ceramic.createDocument('tile', {
  content: { foo: "bar" }
})
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#createdocument)

### Parameters
When creating a document, the first parameter is the [`doctype`](#doctype) and it is always required. Other parameters including [`content`](#content) will vary depending on the *doctype* specification. If you do not specify a *doctype* or if your parameters do not meet the doctype's requirements, your transaction will fail.

#### Doctype
Doctypes are rules that govern the behavior of documents on the Ceramic network.

| Parameter     | Required?   | Value            | Description | Notes |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `doctype`     | required    | string           | Specifies rules for [content](#content), [metadata](#metadata), and conflict resolution |  |

!!! note "Supported doctypes"
    Ceramic currently supports two doctypes: [`tile`]() for storing arbitrary JSON content and [`caip-10 link`]() for storing a proof that binds a DID to a blockchain account. Read each doctype's documentation for more information on its parameters below.

[:octicons-file-code-16: API reference]()

#### DocParams (optional)
DocParams are specifics related to your document including [`content`](#content), [`metadata`](#metadata), and [`deterministic`](#deterministic).

##### Content

| Parameter     | Required?   | Value            | Description | Notes |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `content`     | optional    | object           | The content of your document | Must conform to the [`doctype`](#doctype) and `schema` if present |

!!! note ""
    When `content` is included during document creation, the document's *genesis commit* will be signed by the authenticated user's DID. When `content` is omitted, then the *genesis commit* will not be signed.

[:octicons-file-code-16: API reference]()

##### Metadata

| Parameter     | Required?   | Value               | Description | Notes |
| ------------- | ----------- | ------------------- | ----------- | ----- |
| `schema`      | optional    | string              | URL of a Ceramic document that contains a JSON schema  | If set, schema will be enforced on content |
| `controllers` | optional    | array of strings    | Defines the DID that is allowed to modify the document | If empty, defaults to currently authenticated DID |
| `family`      | optional    | string              | Allows you to group similar documents into *families* | Useful for indexing groups of like documents on the network | 

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.docmetadata-1.html)

##### Deterministic

| Parameter         | Required?   | Value            | Description | Notes |
| ----------------- | ----------- | ---------------- | ----------- | ----- |
| `deterministic`   | optional    | boolean          | If false, allows documents with the same [`doctype`](#doctype), [`content`](#content), and [`metadata`](#metadata) to generate unique DocIDs. | If empty, defaults to false. |

!!! note "Using the `deterministic` parameter"
    For most use cases you will likely want to leave the `deterministic` parameter set to false. However for special circumstances, you may want this to be set to true. For example this should be set to true if you would like to enable [deterministic queries]() for your document. If this is your use case, then it is also important that you omit all [`content`](#content) during document creation. You can proceed to add content to your document by [updating it](#update-a-document).

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.docparams-1.html#deterministic)

#### DocOpts (optional)
DocOpts are options that can be passed to each operation on a document (create, change, load) that control network behaviors performed as part of the operation.  They are not included in the document itself.

| Parameter     | Required?   | Value            | Description | Notes |
| ------------- | ----------- | ---------------- | ----------- | ----- |
| `anchor`      | optional    | boolean          |             |  |
| `publish`     | optional    | boolean          |   | |
| `sync`        | optional    | boolean          | | |
 
[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.docopts-1.html)


## Update a document
Use the [`doc.change()`](https://developers.ceramic.network/reference/javascript/classes/_ceramicnetwork_common.doctype-1.html#change) method to update the [`content`](#content) or [`metadata`](#metadata) of your document. Any update to a document must conform to the update rules specified by the document's [`doctype`](#doctype). Note that you can also pass [`DocOpts`](#docopts-optional) parameters to this method if you wish to not use the default behavior of publishing and/or anchoring your change, but this behavior should be reserved for advanced use cases. 

```javascript
await doc.change({ content: { foo: "updated" }})
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/classes/_ceramicnetwork_common.doctype-1.html#change)

</br>
</br>
</br>
