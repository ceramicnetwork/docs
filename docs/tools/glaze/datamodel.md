# DataModel

DataModels represent a set of Schemas and potentially related Definitions and/or Tiles used together, for example in the context of an application or service built on top of Ceramic.

At runtime, the main purpose of a DataModel, via the `@glazed/datamodel` package, is to provide human-friendly aliases to Stream references. This enables application logic to be abstracted from the underlying references.

During developement, the ModelManager helps keeping track of dependencies between Streams. For example, adding a Definition to a Model will also add the Schema referenced in the Definition to the Model, as both Streams would need to be present on the Ceramic network to work as expected at runtime.

Finally, DataModels promote reusability: a Model can be created and edited on a local node and then deployed to the Ceramic testnet or mainnet with no change required in application code.

## Design

Depending on the context, a DataModel can refer to:

- A **ManagedModel**: an object containing all the data and metadata necessary to represent the Model. This structure is used by the ModelManager and can be serialized to JSON.
- A **PublishedModel**: an object containings human-friendly aliases mapping to Stream references. This structure is used by the DataModel runtime.
- The **DataModel runtime**: a TypeScript library to interact with ModelAliases.

### ManagedModel

A ManagedModel is mostly used as an internal representation for the ModelManager, defined by the following types:

```ts
type ManagedID = string

type ManagedDoc<CommitType = DagJWSResult> = {
  alias: string
  commits: Array<CommitType>
  version: string
}

type ManagedEntry<CommitType = DagJWSResult> = ManagedDoc<CommitType> & {
  schema: ManagedID
}

type ManagedSchema<CommitType = DagJWSResult> = ManagedDoc<CommitType> & {
  dependencies: Record<string, Array<ManagedID>>
}

type ManagedModel<CommitType = DagJWSResult> = {
  schemas: Record<ManagedID, ManagedSchema<CommitType>>
  definitions: Record<ManagedID, ManagedEntry<CommitType>>
  tiles: Record<ManagedID, ManagedEntry<CommitType>>
}
```

Here a `ManagedID` is simply the string value of a `StreamID`.

A ManagedDoc contains the common properties used for any Stream stored in the Model:

- `alias`: the human-friendly alias for the Stream in the Model
- `commits`: the Stream commits, used to publish the Stream to any network
- `version`: the `CommitID` of the Stream

For Definitions and Tiles, an additional `schema` property contains the `ManagedID` of the referenced Schema in the Model, while the ManagedSchema contains `dependencies` on the other Schemas in the models that are referenced by the given Schema.

### PublishedModel

A PublishedModel only serves at runtime to associate human-friendly aliases to Stream references, as defined in the following type:

```ts
type PublishedModel = {
  schemas: Record<string, string>
  definitions: Record<string, string>
  tiles: Record<string, string>
}
```

The key of each Record is the alias while the value is the Stream reference (Commit URL for schemas and Stream ID for definitions and tiles).

### DataModel runtime

The DataModel runtime can be installed from npm:

```sh
npm install @glazed/datamodel
```

Its usage requires a Ceramic instance and a PublishedModel object:

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

## Limitations

- The DataModel tools and libraries are new and will likely go through breaking changes before stable versions are released.
- Only Streams having all commits controlled by a Key DID (using the `did:key` method) are currently supported in DataModels.
- There is no update mechanims for Schema and Definitions yet, but this is something the tools should support over time.

## Lifecycle

### Creation and edition

During development, using the Glaze CLI or a custom script leveraging the ModelManager can be used to create and edit models.

You can read more about these options in the [development page](development.md).

### Publication

Models can be published using the `model:publish` command of the Glaze CLI or calling the `toPublished()` method of a ModelManager instance.

You can see these two options being used in the [example page](example.md#4-deployment-for-runtime).

### Runtime interactions

The `@glazed/datamodel` package exports the `DataModel` class to interact with a model at runtime.

[API reference](../../reference/glaze/classes/datamodel.DataModel.md){: .md-button .md-button .md-button--primary } [Example usage](example.md#5-runtime-usage){: .md-button }

<br /><br /><br />
