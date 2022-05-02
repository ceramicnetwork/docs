# Glaze CLI

## Purpose

The Glaze CLI allows to interact with Ceramic nodes, notably to help support DataModel lifecycle operations, such as the creation, edition and deployment of models.

## Installation

```sh
npm install --global @glazed/cli
```

The CLI is then accessible as `glaze`. Run `glaze help` to list the available commands.

## Common use-cases

### Create a key DID

CLI commands performing stream mutations require the 32-byte hexadecimal-encoded seed of a key DID to be provided, either as the `--key` flag or the `DID_KEY` environment variable:

```sh
glaze did:create
```

The expected output will be similar to the following, with `...` used as placeholder for brievety:

```sh
✔ Created DID did:key:z6Mk... with seed ab...f0
```

The seed can then be used when running other commands:

```sh
glaze [command] --key=ab...f0
DID_KEY=ab...f0 glaze [command]
```

### Create a local model

Creating a local model can be done using the `model:create` command:

```sh
glaze model:create my-model
```

Note that this model is only stored on your local file system, it will not be available externally.

### Add a schema to a local model

Schemas can be added to the model, either by using a Stream already present on the given Ceramic node:

=== "Using an existing schema"

    ```sh
    glaze model:add my-model schema MySchema ceramic://k2...ab
    ```

=== "Creating a new schema"

    ```sh
    glaze model:add my-model schema MySchema '{"$schema":"http://json-schema.org/draft-07/schema#","title":"MySchema","type":"object","properties":{}}' --key=ab...f0
    ```

The `model:add` command can be used in a similar way to add definitions (`model:add my-model definition ...`) and tiles (`model:add my-model tile ...`).

### Import an existing model

Existing models can be imported in a local model either using JSON files or locally installed packages (that can be imported by Node) using the `model:import` command:

=== "Using a local file"

    ```sh
    glaze model:import my-model ./model-to-import.json
    ```

=== "Using a package name"

    ```sh
    glaze model:import my-model package-name-of-model
    ```

### Export a local model

A local model can be exported to a JSON file using the `model:export` command:

```sh
glaze model:export my-model ./my-model.json
```

### Deploy a model to Ceramic

The `model:deploy` command can be used to deploy all the streams used by a given model:

=== "Deploying a local model"

    ```sh
    glaze model:deploy my-model
    ```

=== "Deploying a model file"

    ```sh
    glaze model:deploy ./my-model.json
    ```

=== "Deploying a model package"

    ```sh
    glaze model:deploy package-name-of-model
    ```

Optionally, a second argment can be provided to output the deployed model object to a JSON file:

```sh
glaze model:deploy my-model ./deployed-model.json
```

This output file can then be used by the [DataModel runtime](modules/datamodel.md#datamodel-runtime).

### Deploy IDX models

In this example, we will import and deploy the datamodels defined by the [CIP-19 "Basic Profile"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md), [CIP-21 "Crypto Accounts"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md) and [CIP-23 "Also Known As"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md) specifications.

First, we need to install these datamodels using npm:

```sh
npm install --dev @datamodels/identity-profile-basic @datamodels/identity-accounts-crypto @datamodels/identity-accounts-web
```

We can then import them when using the CLI in a folder where these npm packages can be resolved from, using the steps described in other examples above:

```sh
glaze model:create idx
glaze model:import idx @datamodels/identity-profile-basic
glaze model:import idx @datamodels/identity-accounts-crypto
glaze model:import idx @datamodels/identity-accounts-web
glaze model:deploy idx
```
