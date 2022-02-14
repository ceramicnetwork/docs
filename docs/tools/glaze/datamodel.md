# **Aliasing data models**

The Glaze DataModel library provides human-readable name aliasing for data models, making it easier to refer to and use data models with the DID DataStore API, and other places within your Ceramic application.

## **How it works**

---

The Glaze DataModel library requires an instance of Ceramic and a preexisting `publishedModel` object, which can be created during development using either the [Glaze CLI]() or the [Glaze DevTools]() library.

The primary purpose of the DataModel library is to create a new `PublishedModel` object which contains human-friendly aliases for your data models that can be used at runtime to simplify development. This `PublishedModel` is defined by the following type:

```ts
type PublishedModel = {
  schemas: Record<string, string>
  definitions: Record<string, string>
  tiles: Record<string, string>
}
```

In this object, the key of each record is your alias and the value is a reference to a stream, which for schemas is a commitURL and for definitions and other related tiles is a streamID.


## **Getting started with Glaze DataModel**

---

Visit the [**Glaze DataModel reference →**](../../reference/glaze/classes/datamodel.DataModel.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with the Glaze DataModel:


### **Installation**

```sh
npm install @glazed/datamodel
```

### **Usage**

After you have a `PublishedModel` object from the Glaze CLI or the DevTools library, the main purpose of the Glaze DataModel library is to abstract away the underlying machine-readable names of your schemas and data model definitions into something human-readable that simplifies application development:

```ts
import { DataModel } from '@glazed/datamodel'

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

model.getSchemaURL('MySchema') // 'ceramic://mySchemaURL'
```

## **Next steps: Storing and retrieving data**

---

Your human-readable data model aliases can now be used with the [**DID DataStore**]() module provided by Glaze suite, which is also included in the [**Self.ID SDK**](), to store and retrieve data from these data models.