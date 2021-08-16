# Development

The Glaze development tools help support DataModel lifecycle operations, such as the creation, edition and publication of models.

## DevTools library

The DevTools library exports the `ModelManager` class, that can be used in scripts to programmatically manage a model.

### Installation

```sh
npm install --dev @glazed/devtools
```

### Usage

The `ModelManager` class is exported by the `@glazed/devtools` package.

[API reference](../../reference/glaze/classes/devtools.ModelManager.md){: .md-button .md-button .md-button--primary }

### Creation and edition

The `ModelManager` constructor requires a Ceramic instance, that must be authenticated when calling the creation methods (`createSchema()`, `createDefinition()` and `createTile()`).

```ts
import { ModelManager } from '@glazed/devtools'

const manager = new ModelManager(ceramic)
```

Schemas can be added to the model, either by using a Stream already present on the given Ceramic node:

```ts
await manager.usePublishedSchema('MySchema', schemaStreamReference)
```

Or by creating a new one:

```ts
await manager.createSchema('MySchema', {
  $schema: 'http://json-schema.org/draft-07/schema#',
  title: 'MySchema',
  type: 'object',
  properties: {
    ...
  },
})
```

Similar methods can be used to add Definitions (`usePublishedDefinition()` and `createDefinition()`) or Tiles (`usePublishedTile()` and `createTile()`).

### Import and export

The ManagedModel used internally by a `ModelManager` can be exported and imported as JSON for easier portability and storage:

```ts
const encodedModel = manager.toJSON()

const clonedManager = ModelManager.fromJSON(ceramic, encodedModel)
```

### Deployment

The model can be deployed to the Ceramic node attached to the manager by calling the `toPublished()` method:

```ts
const publishedModel = await manager.toPublished()
```

## CLI

### Installation

```sh
npm install --global @glazed/cli
```

The CLI is then accessible as `glaze`.

### Usage

Run `glaze help` to list the available commands. The commands for some common flows are described below.

### Creation and edition

In order to create streams, we need a Key DID. If you don't already have one, you can use the `did:create` command:

```sh
glaze did:create
```

Creating a local model can be done using the `model:create` command:

```sh
glaze model:create my-model
```

Schemas can be added to the model, either by using a Stream already present on the given Ceramic node:

=== "Using an existing schema"

    ```sh
    glaze model:add my-model schema MySchema schemaStreamReference
    ```

=== "Creating a new schema"

    ```sh
    glaze model:add my-model schema MySchema '{"$schema":"http://json-schema.org/draft-07/schema#","title":"MySchema","type":"object","properties":{}}' --key=<your key>
    ```

The `model:add` command can be used in a similar way to add Definitions (`model:add my-model definition ...`) and Tiles (`model:add my-model tile ...`).

### Import and export

Existing models can be imported in a local model either using JSON files or locally installed packages (that can be imported by Node) using the `model:import` command:

=== "Using a local file"

    ```sh
    glaze model:import my-model ./model-to-import.json
    ```

=== "Using a package name"

    ```sh
    glaze model:import my-model package-name-of-model
    ```

A local model can also be exported to a JSON file using the `model:export` command:

```sh
glaze model:export my-model ./my-model.json
```

### Deployment

The `model:publish` command can be used to publish all the Streams used by a given model:

=== "Publishing a local model"

    ```sh
    glaze model:publish my-model
    ```

=== "Publishing a model file"

    ```sh
    glaze model:publish ./my-model.json
    ```

=== "Publishing a model package"

    ```sh
    glaze model:publish package-name-of-model
    ```

Optionally, a second argment can be provided to output the [PublishedModel](datamodel.md#publishedmodel) to a JSON file:

```sh
glaze model:publish my-model ./published-model.json
```

This output file can then be used by the [DataModel runtime](datamodel.md#datamodel-runtime).

<br /><br /><br />
