# Glaze example

## 1. Prerequisites

In this example, we'll create a DataModel for a simple text note that can be associated to a DID, and an associated script to interact with this model.

First, we will need to install the following runtime packages:

```sh
npm install @ceramicnetwork/http-client @glazed/datamodel @glazed/did-datastore dids key-did-provider-ed25519 key-did-resolver uint8arrays
```

The following steps will present two ways of going through the development flow: creating a custom script (recommended option for complex models) or using the Glaze CLI (simpler option for basic models):

=== "Using a script"

    ```sh
    npm install --dev @glazed/devtools
    ```

=== "Using the CLI"

    ```sh
    npm install --global @glazed/cli
    ```

## 2. Local setup

=== "Script"

    In order to create a DataModel, we need a Ceramic instance with an authenticated DID.
    DataModels only support DIDs using the `did:key` method, so we'll `key-did-provider-ed25519` in this example.

    Let's create a `create-model.mjs` file with the following contents:

    ```js
    import { writeFile } from 'node:fs/promises'
    import { CeramicClient } from '@ceramicnetwork/http-client'
    import { ModelManager } from '@glazed/devtools'
    import { DID } from 'dids'
    import { Ed25519Provider } from 'key-did-provider-ed25519'
    import { getResolver } from 'key-did-resolver'
    import { fromString } from 'uint8arrays'

    // The key must be provided as an environment variable
    const key = fromString(process.env.DID_KEY, 'base16')
    // Create and authenticate the DID
    const did = new DID({
      provider: new Ed25519Provider(key),
      resolver: getResolver(),
    })
    await did.authenticate()

    // Connect to the local Ceramic node
    const ceramic = new CeramicClient('http://localhost:7007')
    ceramic.did = did

    // Create a manager for the model
    const manager = new ModelManager({ ceramic })
    ```

=== "CLI"

    Creating a local DataModel using the CLI simply requires to run the `model:create` command with a name to identify the model:

    ```sh
    glaze model:create simple-note
    ```

    Let's also create a DID if you don't already have one:

    ```sh
    glaze did:create
    ```

    You can then set the displayed key as the `DID_KEY` environment variable when running other commands, or make sure to provide it using the `--key` flag, as used in the following steps.

## 3. DataModel creation

### SimpleNote schema

We'll first create a JSON schema for an object containing a `text` property.

=== "Script"

    ```js
    const noteSchemaID = await manager.createSchema('SimpleNote', {
      $schema: 'http://json-schema.org/draft-07/schema#',
      title: 'SimpleNote',
      type: 'object',
      properties: {
        text: {
          type: 'string',
          title: 'text',
          maxLength: 4000,
        },
      },
    })
    ```

=== "CLI"

    Here we will use the `model:add` command, that takes the following 4 arguments:

    1. The local model name
    2. The type to add, here a `schema`
    3. The alias for this schema, here `SimpleNote`
    4. The schema contents, as a JSON-encoded string

    ```sh
    glaze model:add simple-note schema SimpleNote '{"$schema":"http://json-schema.org/draft-07/schema#","title":"SimpleNote","type":"object","properties":{"text":{"type":"string","title":"text","maxLength":4000}}}' --key=<your key>
    ```

Now that we have a schema, we can use it as reference in the following steps.

### myNote definition

Definitions need to reference a specific version of a schema using a commit ID in URL form (starting with `ceramic://`).

=== "Script"

    Using the manager, schema URLs should be accessed using the `getSchemaURL()` method:

    ```js
    // Create the definition using the created schema ID
    await manager.createDefinition('myNote', {
      name: 'My note',
      description: 'A simple text note',
      schema: manager.getSchemaURL(noteSchemaID),
    })
    ```

=== "CLI"

    Before we can create a definition, we need to inspect the model to retrieve the commit ID of our schema, identified by the `version`:

    ```sh
    glaze model:inspect simple-note
    ```

    We can then copy the `version` associated to the `SimpleNote` schema to the following command:

    ```sh
    glaze model:add simple-note definition myNote '{"name":"My note","description":"A simple text note","schema":"ceramic://<SimpleNote schema version ID>"}' --key=<your key>
    ```

### exampleNote tile

Creating a tile is similar to a definition, except for the `schema` reference that needs to be provided as metadata rather than in the content:

=== "Script"

    ```js
    // Create a tile using the created schema ID
    await manager.createTile('exampleNote',
      { text: 'A simple note' },
      { schema: manager.getSchemaURL(noteSchemaID) },
    )
    ```

=== "CLI"

    ```sh
    glaze model:add simple-note tile exampleNote '{"text":"A simple note"}' --schema='ceramic://<SimpleNote schema version ID>' --key=<your key>
    ```

## 4. Deployment for runtime

In this example, we are creating streams on the local Ceramic node, so we know they are already present and accessible at runtime.
There are various cases however when we can't make assumptions about the presence of the necessary streams, for example on a testnet or CI node.

The deployment process consists in publishing all the necessary streams potentially used by our DataModel at runtime to a given Ceramic node, in order to ensure their availability.
The deployed model is then written to the `model.json` file, that will simply contain mappings for our aliases to stream references, for example:

```json
{
  "definitions": {
    "myNote": "kjzl6cwe1jw145a8rcoo8y9c1jtwa71cc33vvz7gqah1tg4dvoxkd62u8mzbz4d"
  },
  "schemas": {
    "SimpleNote": "ceramic://k3y52l7qbv1frxtwv4ip9pdwe6vh3dh13rs0ynh4pwwqo7usnudlsvgm8w2gl4xz4"
  },
  "tiles": {
    "exampleNote": "kjzl6cwe1jw149ymdkfpkmf0ighksbfxfgt6bm4ut45mapn87a4wm9nkf7xouzg"
  }
}
```

=== "Script"

    We can add the following code at the end of our `create-model.mjs` script:

    ```js
    // Deploy model to Ceramic node
    const model = await manager.deploy()

    // Write deployed model aliases to JSON file
    await writeFile('./model.json', JSON.stringify(model))
    ```

=== "CLI"

    We can use the `model:deploy` command to deploy a model, optionally with a second argument to output the deployed model aliases to a JSON file:

    ```sh
    glaze model:deploy simple-note ./model.json
    ```

## 5. Runtime usage

### Runtime setup

Let's create a `run.mjs` file containing the following code:

```js
import { CeramicClient } from '@ceramicnetwork/http-client'
import { DataModel } from '@glazed/datamodel'
import { DIDDataStore } from '@glazed/did-datastore'
import { DID } from 'dids'
import { Ed25519Provider } from 'key-did-provider-ed25519'
import { getResolver } from 'key-did-resolver'
import { fromString } from 'uint8arrays'

// Import the model aliases created during development time
import modelAliases from './model.json'

// The key must be provided as an environment variable
const key = fromString(process.env.DID_KEY, 'base16')
// Create and authenticate the DID
const did = new DID({
  provider: new Ed25519Provider(key),
  resolver: getResolver(),
})
await did.authenticate()

// Create the Ceramic instance and inject the DID
const ceramic = new CeramicClient('http://localhost:7007')
ceramic.did = did

// Create the model and store
const model = new DataModel({ ceramic, aliases: modelAliases })
const store = new DIDDataStore({ ceramic, model })
```

This setup can then be used to perform any of the following use-cases.

### Known note loading

We can now load the `exampleNote` tile using its alias:

```js
const exampleNote = await model.loadTile('exampleNote')
```

### New note creation

Using the `DataModel` runtime, it is also possible to create new notes using the schema alias:

```js
const newNote = await model.createTile('SimpleNote', { text: 'My new note' })
```

### Note associated to a DID

Finally, using the DID DataStore, we can interact with the `myNote` record associated to a given DID:

```js
await store.set('myNote', { text: 'This is my note' })

await store.get('myNote') // { text: 'This is my note' }
```
