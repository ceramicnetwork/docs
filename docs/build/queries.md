# Queries
Query documents on the Ceramic network. The examples on this page describes how
to make transactions using the [http]() and [core]() clients.
You can also use the CLI using the examples on the [Quick Start](quick-start.md)
page.


## Prerequisites
You need to have an [installed client](installation.md) to perform queries.

## Query a single document
Use the `loadDocument()` method to load a single document using its *DocID*.

``` javascript
const docid = 'kjzl6cwe1jw14...'
const doc = await ceramic.loadDocument(docid)
```

[:octicons-file-code-16: API reference]()

## Query a document using its genesis commit
One the [Transactions](transactions.md#create-a-document-deterministically) page
we saw how to create an unsigned document deterministically. We can query such
a document in the same way as it was created.

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
Use the `multiQuery()` method to load multiple documents at once.

``` javascript
const queries = [{
  docId: 'kjzl6cwe1jw14...'
}, {
  docId: 'kjzl6cwe1jw15...'
}]
const docMap = await ceramic.multiQuery(queries)
```

The returned object `docMap` is a map from *DocID* to document instance.

[:octicons-file-code-16: API reference]()

## Query a document path
Use the `multiQuery()` method to load a document using a path through linked
documents.

If the content of a document with *DocID* `kjzl6cwe1jw14...` is,

``` javascript
{
  a: 'kjzl6cwe1jw15...',
  b: { c: 'kjzl6cwe1jw16...' }
}
```
the multiQuery below would return a map with three documents.

``` javascript
const queries = [{
  docId: 'kjzl6cwe1jw14...'
  paths: ['/a', '/b/c']
}]
const docMap = await ceramic.multiQuery(queries)
```
[:octicons-file-code-16: API reference]()
