# **DID DataStore**

---

DID DataStore is a runtime library that allows any application to store and retrieve data from a Ceramic account's personal datastore, with support for public and private data. The DID DataStore API is based on data models; when multiple applications reuse the same data model, they reuse the same underlying data. As such, DID DataStore is core to how Ceramic delivers user-centric data composability across applications.

## **How it works**

---
The DID DataStore is an implementation of the [Identity Index (IDX) protocol](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-11/CIP-11.md), allowing to associate records to a DID. To learn more about the IDX protocol, see its [reference documentation]().

![Image that describes the architecture of the Identity Index protocol](../../images/idx-architecture.png)

## **Getting started with DID DataStore**

---

Visit the [**DID DataStore reference →**](../../reference/glaze/classes/did_datastore.DIDDataStore.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with DID DataStore:

### **Installation**

```sh
npm install @glazed/did-datastore
```

### **Usage**

The DID DataStore requires a Ceramic instance and either a [DataModel instance](datamodel.md#datamodel-runtime) or [PublishedModel object](datamodel.md#publishedmodel) provided by the Glaze DataModel library:

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

### **Read/Write Data**

It is then possible to read and write with data in the user's DataStore:

```ts
await dataStore.set('myDefinition', { record: 'content' })

await dataStore.get('myDefinition') // { record: 'content' }
```