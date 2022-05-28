# **Deploying data models with the Glaze CLI**

---

The Glaze CLI provide commands for common interactions with data models to help support your development workflow.

> The Glaze CLI is useful for basic data model management. For managing multiple models or more complex models, we recommend using the Glaze [DevTools](development.md) library.

## **Getting started with the Glaze CLI**

---

### **Installation**

```sh
npm install --global @glazed/cli
```

The CLI is then accessible as `glaze`.

### **Usage**

Run `glaze help` to list the available commands. The commands for some common flows are described below.

### **Set up your developer account**

In order to create streams that store the schemas and definitions for our data models, you'll need to use a Key DID account. If you don't already have one, you can use the `did:create` command. It will output a random hexademical `seed` that can be used as the `DID_KEY` environment variable or as the `--key` flag in the following commands.

```sh
glaze did:create
```

### **Creating data models**

Creating a local model can be done using the `model:create` command:

```sh
glaze model:create my-model
```

Schemas can be added to the model, either by using a stream already present on the given Ceramic node or by creating a new one:

=== "Using an existing schema"

    ```sh
    glaze model:add my-model schema MySchema schemaStreamReference
    ```

=== "Creating a new schema"

    ```sh
    glaze model:add my-model schema MySchema '{"$schema":"http://json-schema.org/draft-07/schema#","title":"MySchema","type":"object","properties":{}}' --key=<your key>
    ```

The `model:add` command can be used in a similar way to add Definitions (`model:add my-model definition ...`) and Tiles (`model:add my-model tile ...`).

### **Import data models**

Existing models can be imported in a local model either using JSON files or locally installed packages (that can be imported by Node) using the `model:import` command:

=== "Using a local file"

    ```sh
    glaze model:import my-model ./model-to-import.json
    ```

=== "Using a package name"

    ```sh
    glaze model:import my-model package-name-of-model
    ```

### **Export data models as JSON**

A local model can also be exported to a JSON file using the `model:export` command:

```sh
glaze model:export my-model ./my-model.json
```

### **Deploy to Ceramic**

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

Optionally, a second argment can be provided to output the [ModelAliases](datamodel.md) to a JSON file:

```sh
glaze model:deploy my-model ./deployed-model.json
```

## **Next steps: Using models in your app**

---

### **Aliasing your data models**

Your output file can be used by the [DataModel](datamodel.md#usage) module contained in Glaze suite in order to give your model a friendly, human-readable name.

### **Storing and retrieving data**

Your data models are now available to be used by the DID DataStore module provided by Glaze suite, which is also included in the Self.ID framework, to store and retrieve data from these data models.

## **Example**

---

In this example, we will import and deploy popular Ceramic data models found in the [Data Models Registry](../../docs/advanced/standards/data-models/data-model-universe.md), defined by the [CIP-19 "Basic Profile"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md), [CIP-21 "Crypto Accounts"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md) and [CIP-23 "Also Known As"](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md) specifications.

First, we need to install these data models from the Data Models Registry using npm:

```sh
npm install --dev @datamodels/identity-profile-basic @datamodels/identity-accounts-crypto @datamodels/identity-accounts-web
```

We can then import them when using the CLI in a folder where these npm packages can be resolved from, using the steps described above:

```sh
glaze model:create idx
glaze model:import idx @datamodels/identity-profile-basic
glaze model:import idx @datamodels/identity-accounts-crypto
glaze model:import idx @datamodels/identity-accounts-web
glaze model:deploy idx
```
