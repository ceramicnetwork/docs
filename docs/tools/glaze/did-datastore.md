# DID DataStore

The DID DataStore is an implementation of the [Identity Index (IDX) CIP](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-11/CIP-11.md), allowing to associate records to a DID.

## Design

Dive into the design and architecture of the IDX protocol, which is implemented by the DID DataStore.

![Image that describes the architecture of IDX](../../images/idx-architecture.png)

### Index

The index is a stream controlled by the user's DID which stores entries consisting of [definition](#definitions) (represented by a streamID) to [record](#records) (represented by a streamID) mappings. Every DID has only one global index and its entries represent the entire catalog of data that belongs to the user. An index is similar to a row in a user table, and enables the decentralized association and discovery of streams that belong to a user.

Example:

```js
{
  "kyz123...456": "ceramic://kyz789...012",
  "kyz345...678": "ceramic://kyz901...234",
  "kyz567...890": "ceramic://kyz123...456",
  "kyz789...012": "ceramic://kyz345...678"
}
```

### Definitions

Definitions are streams created by application developers that store metadata which describes the stream used for data storage. Definitions allow records to be semantically described and queried using their metadata or [schema](#schemas) and are similar to a column in a user table. The streamID of the definition is a key in the [index](#index).

Example:

```js
{
  name: 'Basic Profile',
  description: 'A simple basic profile.',
  schema: 'ceramic://kyz123...456'
}
```

### Schemas

Schemas are streams created by application developers that store a JSON schema. They specify the data schema of a [record](#records). Schemas are identified by the streamID of the stream that stores the schema, which is included in the [definition](#definitions) as seen above.

### Records

Records are streams that store information for a DID. They can directly store content, or they can store foreign key references to external datastores outside of Ceramic. A record is similar to a cell in a user table. The streamID of the record is a value in the [index](#index).

Example:

```js
{
  name: 'Alan Turing',
  description: 'I make computers beep good.',
  emoji: '💻'
}
```

## Installation

```sh
npm install @glazed/did-datastore
```

## Usage

The DID DataStore requires a Ceramic instance and either a [DataModel instance](datamodel.md#datamodel-runtime) or [PublishedModel object](datamodel.md#publishedmodel):

=== "Using a DataModel instance"

    ```ts
    import { DataModel } from '@glazed/datamodel'
    import { DIDDataStore } from '@glazed/did-datastore'

    const publishedModel = {
      schemas: {
        MySchema: 'ceramic://mySchemaURL',
      },
      definitions: {
        myDefinition: 'myDefinitionID',
      },
      tiles: {},
    }

    const model = new DataModel({ ceramic, model: publishedModel })
    const dataStore = new DIDDataStore({ ceramic, model })
    ```

=== "Using a PublishedModel directly"

    ```ts
    import { DIDDataStore } from '@glazed/did-datastore'

    const publishedModel = {
      schemas: {
        MySchema: 'ceramic://mySchemaURL',
      },
      definitions: {
        myDefinition: 'myDefinitionID',
      },
      tiles: {},
    }

    const dataStore = new DIDDataStore({ ceramic, model: publishedModel })
    ```

It is then possible to interact with records attached to the DataStore:

```ts
await dataStore.set('myDefinition', { record: 'content' })

await dataStore.get('myDefinition') // { record: 'content' }
```

<br /><br /><br />
