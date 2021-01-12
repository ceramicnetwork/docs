# Transactions
Transactions are interactions that write to Ceramic documents, such as creating
new documents and transforming existing documents. The examples on this page
describes how to make transactions using the [http]() and [core]() clients.
You can also use the CLI using the examples on the [Quick Start](quick-start.md)
page.

## Prerequisites
You need an [installed client](installation.md) and an
[authenticated user](authentication.md) to perform transactions on the network.

## Create a document
Use the `createDocument()` method to create a new document. By default the
controller of the document will be the currently authenticated user.


``` javascript
const doc = await client.createDocument('tile', {
  content: { foo: "bar" }
})
```

[:octicons-file-code-16: API reference]()

## Create a document deterministically
By default the first *genesis commit* of a document is signed. Sometimes it's
useful to be able to create a document where you can compute the *DocID* using
only the content of the *genesis commit*. In order to do this we can specify
the `deterministic` flag. Important to note is that if we want the *genesis
commit* to not be signed we should not add any `content` property when we create
the document.

The `controllers` property defines which DID that is allowed to make updates
to the document. The `family` property allows you to group similar document into
*families*.


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

??? tip "Parameters"
    When creating a document, the first parameter is the `doctype` and is always
    required. Other parameters and content requirements will vary depending on
    the doctype. If you do not specify a doctype or if your parameters do not
    meet the doctype's requirements, your transaction will fail.

    Read each doctype's documentation for more information on the required and optional parameters.

    - [Tile doctype]()
    - [CAIP-10 link doctype]()

## Update a document
Use the `doc.change()` method to update the content of your document. Any update
to a document must conform to the update rules specified by the document's
doctype.


``` javascript
await doc.change({ content: { foo: "updated" }})
```

Note that you can also specify updates to the `metadata` property.

[:octicons-file-code-16: API reference]()



</br>
</br>
</br>
