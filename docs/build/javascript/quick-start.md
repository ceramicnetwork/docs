# Quickstart
Learn the basics by setting up and interacting with the [Ceramic Javascript Client](./javascript/installation.md).

!!! warning ""
    **Want an even faster way to try Ceramic? Visit the [Playground Demo App](https://playground.ceramic.dev){:target="_blank"} to test the full stack of Ceramic Components in the browser.

## **1. Install the Client**

Visit the [Ceramic Client](./javascript/installation.md) page for instructions on how to quickly install the Client.

## **2. Create a stream**
Creating streams is dependent on the StreamType. More details can be found in our [StreamTypes Overview](../streamtypes/overview.md). A basic sample using the [TileDocument StreamType](../streamtypes/tile-document/overview.md) can be found below.

=== "Command"

    ```JavaScript
    import { TileDocument } from '@ceramicnetwork/stream-tile'

    const schemaId = 'k3y52l7qbv1fry1fp4s0nwdarh0vahusarpposgevy0pemiykymd2ord6swtharcw' // this is our Basic Profile Schema.
    
    const content = {
      name: 'Your Name',
      emoji: '✌🏻',
      description: 'Something about yourself'
    }
    const metadata = {
      controllers: [ceramic.did.id],
      family: 'Basic Profile',
      schema: schemaId
    }

    const doc = await TileDocument.create(ceramic, content, metadata)

    const streamId = doc.id.toString()
    ```

## **3. Query a stream**
Use the `load()` function to query the current [state](../learn/glossary.md#state) of a stream. You will need to provide it's *StreamID*.

=== "Command"

    ```JavaScript
    const streamId = "kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa"
    const stream = await TileDocument.load(streamId)
    ```

## **4. Update a stream**
Use the `update`  command to update a stream. your [DID](../learn/glossary.md#dids) must be in the [controller](../learn/glossary.md#controllers) of the stream in order to update it. 

=== "Command"

    ```JavaScript
    const streamId = "kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa"
    const doc = await TileDocument.load(ceramic, streamId, opts)
    
    await doc.update(newContent, newMeta, opts)
    ```


In the following example we update a TileDocuments' content while also giving it a tag.

=== "Command"

    ```JavaScript
    const streamId = "kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa"
    const doc = await TileDocument.load(ceramic, streamId)

    await doc.update({foo: 'baz'}, {tags: ['baz']})
    ```
<!-- API Reference here -->

## **5. Create a schema**
TileDocuments can enforce that their contents adhere to a specified schema. The schemas themselves are TileDocuments where the content is a [json-schema](https://json-schema.org){:target="_blank"}. For example we can create a schema that requires a TileDocument to have a *title* and a *message*.

=== "Command"

    ```JavaScript
    const schema = {
      "$schema": "http://json-schema.org/draft-07/schema#",
      "title": "Reward",
      "type": "object",
      "properties": {
        "title": { "type": "string" },
        "message": { "type": "string" }
      },
      "required": [
        "message",
        "title"
      ]
    }
    const metadata = {
      controllers: [ceramic.did.id] // this will set yourself as the controller of the schema
    }
    const rewardSchema = await TileDocument.create(ceramic, schema, metadata)
    ```

## **6. Create a TileDocument stream that uses a schema**
First, use the `rewardSchema.commitId.toString()` to get the current [commitIDs](../learn/glossary.md#commitid) of the schema stream. When creating a TileDocument that uses this schema need to use a commitID instead of the StreamID. This is to enforce that we are using a specific version of the schema since the schema stream is mutable and can be updated.

=== "Command"

    ```JavaScript
      const reward = await TileDocument.create(ceramic, {
        title: 'Hello',
        message: 'world!'
      }, {
        controllers: [ceramic.did.id],
        family: 'Rewards',
        schema: rewardSchema.commitId.toString(),
      })
    ```

## **7. Inspect the state of the stream you created**
Use `reward.state` to query the state of the TileDocument we just created. We can see that the schema is set to the correctID.

=== "Command"

    ```JavaScript
      console.log(reward.state)
    ```

=== "Output"

    ```JSON
    {
      anchorStatus: 1,
      content: {
        message: "world!",
        title: "Hello"
      },
      metadata:{
        controllers: [
          "did:3:kjzl...mr5y",
        ]
        family: "Rewards"
        schema: "k3y5...50jk"
        unique: "/mNCNLFt+1a0nqgL"
      }
    }
    ```

# **That's it!**
Congratulations on completing this tutorial! You're well on your way to becoming a Ceramic developer. Now let's [install Ceramic in your project →](./javascript/installation.md)
</br>
</br>
</br>