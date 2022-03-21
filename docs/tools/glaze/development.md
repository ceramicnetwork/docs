# **Deploy data models from JavaScript**

---

The Glaze DevTools module provides JavaScript APIs that allow you to programmatically create, edit, and deploy the data models used by your Ceramic application using a script. As opposed to manually [deploying data models with the Glaze CLI](deploy-from-cli.md), DevTools makes it easy to manage more models or more complex models.

## **How it works**

---

The DevTools module exports a `ModelManager` class, that can be used in scripts to programmatically manage a data model. The ModelManager object contains all data and metadata necessary to represent a Ceramic data model, and can be serialized to JSON.

The ModelManager helps you to keep track of dependencies between streams that comprise your data models during development. For example, adding a data model definition to a model will also add the schema referenced by the definition, as both streams need to be present on your Ceramic node at runtime to work as expected.

Additionally, data models promote reusability: a model can be created and edited on a local node and then deployed to the Ceramic testnet and mainnet with no change required in application code.

## **Getting started with DevTools**

---

Visit the [**Glaze DevTools reference →**](../../reference/glaze/modules/devtools.md) documentation for full instructions on how to install, configure, and use the module in your application.

### **Installation**

```sh
npm install --dev @glazed/devtools
```

### **Setup**

The `ModelManager` constructor requires a Ceramic instance, that must be authenticated when calling the create methods: `createSchema()`, `createDefinition()` and `createTile()`.

```ts
import { ModelManager } from '@glazed/devtools'

const manager = new ModelManager({ ceramic })
```

### **Constructing your model manager**

A model manager consists of various data models used by your application. Each data model consists of one or more schemas and a definition. You'll need to add those schemas and definitions to your model manager.

#### Adding schemas

If the stream containing the schema you wish to use is already available on your Ceramic node, you can add it like this:

```ts
await manager.useDeployedSchema('MySchema', schemaStreamReference)
```

If the schema you wish to use is not available on your Ceramic node or has not been created yet, you can create it like this:

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

#### Adding definitions

Similar methods can be used to add data model definitions with `useDeployedDefinition()` and `createDefinition()` or Tiles with `useDeployedTile()` and `createTile()`.

### **Import/export as JSON**

The ManagedModel used internally by a `ModelManager` can be exported and imported as JSON for easier portability and storage:

```ts
const model = manager.toJSON()

const clonedManager = ModelManager.fromJSON({ ceramic, model })
```

### **Deploy to Ceramic**

The model can be deployed to your Ceramic node by calling the `deploy()` method:

```ts
const modelAliases = await manager.deploy()
```

## **Example**

---

In this example, we will import and publish popular Ceramic data models found in the [Data Models Registry](../../docs/advanced/standards/data-models/data-model-universe.md), defined by the [CIP-19 "Basic Profile"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md), [CIP-21 "Crypto Accounts"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md) and [CIP-23 "Also Known As"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md) specifications.

First, install these data models from the Data Models Registry using npm:

```sh
npm install --dev @datamodels/identity-profile-basic @datamodels/identity-accounts-crypto @datamodels/identity-accounts-web
```

Now we can then use them in a script:

```ts
import { ModelManager } from '@glazed/devtools'
import { model as basicProfileModel } from '@datamodels/identity-profile-basic'
import { model as cryptoAccountsModel } from '@datamodels/identity-accounts-crypto'
import { model as webAccountsModel } from '@datamodels/identity-accounts-web'

const manager = new ModelManager({ ceramic })
manager.addJSONModel(basicProfileModel)
manager.addJSONModel(cryptoAccountsModel)
manager.addJSONModel(webAccountsModel)

const aliases = await manager.deploy()
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

In order to create streams, we need a Key DID. If you don't already have one, you can use the `did:create` command. It will output a random hexademical `seed` that can be used as the `DID_KEY` environment variable or as the `--key` flag in the following commands.

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

### Creating, Updating and Querying Tiles

Using the `tile:*` commands you can interact with any of the tile commands:

=== "Create"
    ```sh
    glaze tile:create -b '{"foo":"bar"}'
    ```

=== "Create Deterministic"
    ```sh
    glaze tile:deterministic '{"family": "CLI"}'
    ```

=== "Updates"
    ```sh
    glaze tile:update <STREAMID> -b '{"foo":"baz"}'
    ```

=== "Query"
    ```sh
    glaze tile:content <STREAMID>
    ```

### Stream Interactions

The `stream:*` commands are used to interact with the history of your tiles. 

=== "Commits"
    ```sh
    glaze stream:commits <STREAMID>
    ```

=== "State"
    ```sh
    glaze stream:state <STREAMID>
    ```

### Pinning Streams

Pinning a stream is used to persist the data that you're creating, the `pin:*` commands are used to add, remove or list pins.

=== "Add"
    ```sh
    glaze pin:add <STREAMID>
    ```

=== "Remove"
    ```sh
    glaze pin:rm <STREAMID>
    ```

=== "List"
    ```sh
    glaze pin:ls
    ```

### Example: using IDX models

In this example, we will import and publish the datamodels defined by the [CIP-19 "Basic Profile"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md), [CIP-21 "Crypto Accounts"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md) and [CIP-23 "Also Known As"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md) specifications.

First, we need to install these datamodels using npm:

```sh
npm install --dev @datamodels/identity-profile-basic @datamodels/identity-accounts-crypto @datamodels/identity-accounts-web
```

We can then import them when using the CLI in a folder where these npm packages can be resolved from, using the steps described above:

```sh
glaze model:create idx
glaze model:import idx @datamodels/identity-profile-basic
glaze model:import idx @datamodels/identity-accounts-crypto
glaze model:import idx @datamodels/identity-accounts-web
glaze model:publish idx
```
