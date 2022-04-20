# **Aliasing data models**

The Glaze DataModel library provides human-readable name aliasing for data models, making it easier to refer to and use data models with the DID DataStore API, and other places within your Ceramic application.

## **How it works**

---

The Glaze DataModel library requires an instance of Ceramic and a preexisting `aliases` object, which can be created during development using either the [Glaze CLI](deploy-from-cli.md) or the [Glaze DevTools](development.md) library.

The primary purpose of the DataModel library is to create a new `ModelAliases` object which contains human-friendly aliases for your data models that can be used at runtime to simplify development. This `ModelAliases` is defined by the following type:

```ts
type ModelAliases = {
  schemas: Record<string, string>
  definitions: Record<string, string>
  tiles: Record<string, string>
}
```

In this object, the key of each record is your alias and the value is a reference to a stream, which for schemas is a commitURL and for definitions and other related tiles is a streamID.

## **Getting started with Glaze DataModel**

---

Visit the [**Glaze DataModel reference →**](../../reference/glaze/modules/datamodel.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with the Glaze DataModel:

### **Installation**

```sh
npm install @glazed/datamodel
```

### **Usage**

After you have a `ModelAliases` object from the Glaze CLI or the DevTools library, the main purpose of the Glaze DataModel library is to abstract away the underlying machine-readable names of your schemas and data model definitions into something human-readable that simplifies application development:

```ts
import { DataModel } from '@glazed/datamodel'

const aliases = {
  schemas: {
    MySchema: 'ceramic://mySchemaURL',
  },
  definitions: {
    myDefinition: 'myDefinitionID',
  },
  tiles: {},
}

const model = new DataModel({ ceramic, aliases })

model.getSchemaURL('MySchema') // 'ceramic://mySchemaURL'
```

## **Next steps: Storing and retrieving data**

---

Your human-readable data model aliases can now be used with the [**DID DataStore**](did-datastore.md) module provided by Glaze suite, which is also included in the [**Self.ID SDK**](../../reference/self-id/index.md), to store and retrieve data from these data models.
