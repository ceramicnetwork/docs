# **TileDocument client**

---

The `stream-tile` module exports a `TileDocument` class used to store and load JSON documents using the [CIP-8 "Tile Document" streamcode](../../docs/advanced/standards/stream-programs/tile-document.md), as well a provide accessors to the contents and metadata of a given version of the stream.

## **Installation**

---

```sh
npm install @ceramicnetwork/stream-tile
```

### **Additional requirements**

- In order to load Tile documents, a [Ceramic client instance](../core-clients/ceramic-http.md) must be available
- To create/update documents, the client must have an [authenticated DID](../core-clients/did-jsonrpc.md)

## **Common usage**

---

### **Load a document**

```ts
// Import the Ceramic and Tile document clients
import { CeramicClient } from '@ceramicnetwork/http-client'
import { TileDocument } from '@ceramicnetwork/stream-tile'

// Connect to a Ceramic node
const ceramic = new CeramicClient()

// The `id` argument can be a stream ID (to load the latest version)
// or a commit ID (to load a specific version)
async function load(id) {
  return await TileDocument.load(ceramic, id)
}
```

### **Create a document**

In order to create a document, an authenticated DID needs to be attached to the Ceramic client instance to enable transactions (signing commits).

The following example uses the [ed25519 Key DID provider](../accounts/key-did.md#ed25519) for simplicity, but creating documents can be done using any supported DID provider.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { TileDocument } from '@ceramicnetwork/stream-tile'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

const ceramic = new CeramicClient()

// `seed` must be a 32-byte long Uint8Array
async function authenticateCeramic(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate the DID with the provider
  await did.authenticate()
  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}

async function createDocument(content) {
  // The following call will fail if the Ceramic instance does not have an authenticated DID
  const doc = await TileDocument.create(ceramic, content)
  // The stream ID of the created document can then be accessed as the `id` property
  return doc.id
}
```

In addition to the stream `content`, the following `metadata` can be set

### **Update a document**

In order to update a document, an authenticated DID needs to be attached to the Ceramic client instance to enable transactions (signing commits).

The following example uses the [ed25519 Key DID provider](../accounts/key-did.md#ed25519) for simplicity, but creating documents can be done using any supported DID provider.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { TileDocument } from '@ceramicnetwork/stream-tile'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

const ceramic = new CeramicClient()

// `seed` must be a 32-byte long Uint8Array
async function authenticateCeramic(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate the DID with the provider
  await did.authenticate()
  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}

async function updateDocument(id, content) {
  // First, we need to load the document
  const doc = await TileDocument.load(ceramic, id)
  // The following call will fail if the Ceramic instance does not have an authenticated DID
  await doc.update(content)
}
```

### **Use JSON schema validation**

<!--
TODO: uncomment once the data models docs are available.

!!! note "Related standard"

    The [Data Models standard](../../docs/advanced/standards/data-models/index.md) can be used to manage a set of related schemas, notably by leveraging the [Glaze suite](../glaze/index.md) of tools. -->

Ceramic nodes support validation of documents using JSON schemas. In order for a document to get validated, a Tile document containing the contents of the JSON schema must be created on the node and referenced in metadata.

In this example, we use the `commitID` of the created schema document rather than the stream ID in order to get an *immutable reference to the specific version on the schema*.
This is particularly useful when using schemas that are controlled by external entities, as using the latest version of the schema (using a stream ID as reference) could lead to breaking changes. For example, the schema document could be updated such as `name` would be replaced by `firstName` and `lastName`, but apps having logic implementing setting the `name` would no longer pass validation.
By using the `commitID` of the schema document, apps are guaranteed that documents will be validated against this exact version of the schema.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { TileDocument } from '@ceramicnetwork/stream-tile'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

const ceramic = new CeramicClient()

// `seed` must be a 32-byte long Uint8Array
async function authenticateCeramic(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate the DID with the provider
  await did.authenticate()
  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}

// This function will create the schema document and return the commit ID of the schema,
// providing an immutable reference to the created version of the schema
async function createSchemaDocument() {
  // The following call will fail if the Ceramic instance does not have an authenticated DID
  const doc = await TileDocument.create(ceramic, {
    $schema: 'http://json-schema.org/draft-07/schema#',
    title: 'MySchema',
    type: 'object',
    properties: {
      name: {
        type: 'string',
        maxLength: 150,
      },
    },
    required: ['name'],
  })
  // The stream ID of the created document can then be accessed as the `id` property
  return doc.commitId
}

async function createDocument(content, schema) {
  // The following call will fail if the Ceramic instance does not have an authenticated DID
  const doc = await TileDocument.create(ceramic, content, { schema })
  // The stream ID of the created document can then be accessed as the `id` property
  return doc.id
}

// The following example flow creates the schema and the document using the schema with the same
// DID, in practice it is likely the schemas are created by developers earlier in the development
// flow and the commit IDs of the schemas are referenced by applications at runtime
async function run(seed) {
  await authenticateCeramic(seed)
  const schemaID = await createSchemaDocument()
  const docID = await createDocument({ name: 'Alice' }, schemaID)
}
```

### **Access a deterministic document**

!!! note "Related standard"

    The [CIP-11 "Identity Index" (IDX) standard](../../docs/advanced/standards/application-protocols/identity-index.md) leverages deterministic documents to associate records to a DID and is implemented by the [DID DataStore library](../glaze/modules/did_datastore.md).

Ceramic allows the creation and load of documents based on their [metadata](../../docs/advanced/standards/stream-programs/tile-document.md#metadata). This is useful to identify documents based on their controller and family or tags rather than having to know their stream IDs.

```ts
import { CeramicClient } from '@ceramicnetwork/http-client'
import { TileDocument } from '@ceramicnetwork/stream-tile'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'

const ceramic = new CeramicClient()

// `seed` must be a 32-byte long Uint8Array
async function authenticateCeramic(seed) {
  const provider = new Ed25519Provider(seed)
  const did = new DID({ provider, resolver: getResolver() })
  // Authenticate the DID with the provider
  await did.authenticate()
  // The Ceramic client can create and update streams using the authenticated DID
  ceramic.did = did
}

// Load (or create) a determinitic document for a given controller
async function loadDocumentByController(controller) {
  return await TileDocument.deterministic(ceramic, {
    // A single controller must be provided to reference a deterministic document
    controllers: [controller],
    // A family or tag must be provided in addition to the controller
    family: 'myFamily',
    tags: ['foo'],
  })
}

// The following flow authenticates a DID based on the provided seed and create a deterministic
// document associated to it
async function setMyDocument(seed) {
  await authenticateCeramic(seed)
  // Load the document controlled the authenticated DID
  const doc = await loadDocumentByController(ceramic.did.id)
  // The document has no content as it's created based on metadata only...
  if (doc.content == null) {
    // ... but it can be updated by its controller to set content like any other document
    await doc.update({ hello: 'world' })
  }
}
```

<!--
## Additional Resources

---

- [CIP-8: Tile Document Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-8/CIP-8.md)
- [Complete TileDocument.js API Reference]()

## Next Steps

---

- [Add decentralized indexing so you don't need to keep track of streamIDs between sessions]()
-->
