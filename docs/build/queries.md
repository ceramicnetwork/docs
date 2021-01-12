# Queries
This guide demonstrates how to query documents on the Ceramic network during runtime using the [HTTP]() and [core]() clients. You can also use the CLI to query documents as shown in the [Quick Start](quick-start.md) guide.


## Prerequisites
You need to have an [installed client](installation.md) to perform queries during runtime.

## Query a document
Use the [`loadDocument()`](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#loaddocument) method to load a single document using its *DocID*.

``` javascript
const docid = 'kjzl6cwe1jw14...'
const doc = await ceramic.loadDocument(docid)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.ceramicapi-1.html#loaddocument)

## Query a deterministic document
One the [transactions](transactions.md) page, we discussed how to create a document with the [`deterministic`]() parameter set to true. Since this parameter allows us to generate the exact same DocID if we create two documents with the same [`doctype`]() and [`DocParams`](), it is possible to "query" the document using the same [`createDocument()`]() method that we used to initially create it. Note we are setting the [`DocOpts`]() parameters (`anchor` and `publish`) to false so that we are only loading the document and not publishing any changes to the network.

``` javascript
const doc = ceramic.createDocument('tile', {
  deterministic: true,
  metadata: {
    controllers: ['did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H'],
    family: 'example family'
  },
  { anchor: false, publish: false }
})
```

[:octicons-file-code-16: API reference]()


## Query multiple documents
Use the [`multiQuery()`](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.multiquery-1.html) method to load multiple documents at once. The returned object `docMap` is a map from *DocIDs* to document instances.

```javascript
const queries = [{
  docId: 'kjzl6cwe1jw...14'
}, {
  docId: 'kjzl6cwe1jw...15'
}]
const docMap = await ceramic.multiQuery(queries)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.multiquery-1.html)

## Query document paths
Use the [`multiQuery()`](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.multiquery-1.html) method to load one or more documents using known paths from a root document to its linked documents.

Imagine a document `kjzl6cwe1jw...14` whose content contains the DocIDs of two other documents. These DocIDs exist at various levels within a nested JSON structure. 

```javascript
{
  a: 'kjzl6cwe1jw...15',
  b: { 
    c: 'kjzl6cwe1jw...16'
  }
}
```

In the document above, the path from root document `kjzl6cwe1jw...14` to linked document `kjzl6cwe1jw...15` is `/a` and the path to linked document `kjzl6cwe1jw...16` is `/b/c`. Using the DocID of the root document and the paths outlined here, we use `multiQuery()` to query all three documents at once without needing to explicitly know the DocIDs of the two linked documents.

The `multiQuery()` below will return a map with all three documents.

``` javascript
const queries = [{
  docId: 'kjzl6cwe1jw...14'
  paths: ['/a', '/b/c']
}]
const docMap = await ceramic.multiQuery(queries)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/javascript/interfaces/_ceramicnetwork_common.multiquery-1.html)
