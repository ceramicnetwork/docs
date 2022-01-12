# Quickstart

Learn the basics by setting up and interacting with the [Ceramic Javascript Client](./installation.md).

!!! warning ""
**Want an even faster way to try Ceramic?** Visit the [Playground Demo App](https://playground.ceramic.dev){:target="\_blank"} to test the full stack of Ceramic components in the browser.

## **1. Install the Client**

!!! warning "Authentication"
:octicons-alert-16: If you're using this quickstart anywhere but the Ceramic Playground you'll need to authenticate your Ceramic instance. This is a process that's dependent on your setup so we recommend taking a look at our [authentication section](./authentication.md){:target="\_blank"} to ensure you don't have any issues following along.

Visit the [Ceramic Client](./installation.md) page for instructions on how to quickly install the Client.

## **2. Create a stream**

Creating streams is dependent on the StreamType. More details can be found in our [StreamTypes Overview](../../docs/advanced/standards/stream-programs/index.md). A basic sample using the [TileDocument StreamType](../../docs/advanced/standards/stream-programs/cip8-tile-document.md) can be found below.

=== "Command"

    ```JavaScript
    import { TileDocument } from '@ceramicnetwork/stream-tile'

    const doc = await TileDocument.create(ceramic, {hello: 'world'})

    console.log(doc.content)

    const streamId = doc.id.toString()
    ```

=== "Output"

    ```JavaScript
    {
      hello: 'world'
    }
    ```

## **3. Query a stream**

Use the `load()` function to query the current [state](../../learn/glossary.md#state) of a stream. You will need to provide it's _StreamID_.

=== "Command"

    ```JavaScript
    const streamId = "kjzl...g8qa"
    const doc = await TileDocument.load(ceramic, streamId)

    console.log(doc.content)
    ```

=== "Output"

    ```JavaScript
    ```

## **4. Update a stream**

Use the `update` command to update a stream. your [DID](../../learn/glossary.md#dids) must be in the [controller](../../learn/glossary.md#controllers) of the stream in order to update it.

=== "Command"

    ```JavaScript
    const streamId = "kjzl...g8qa"
    const doc = await TileDocument.load(ceramic, streamId)

    await doc.update(newContent, newMeta)
    ```

In the following example we update a TileDocuments' content while also giving it a tag.

=== "Command"

    ```JavaScript
    const streamId = "kjzl...g8qa"
    const doc = await TileDocument.load(ceramic, streamId)

    await doc.update({foo: 'baz'}, {tags: ['baz']})
    ```

## **5. Create a schema**

TileDocuments can enforce that their contents adhere to a specified schema. The schemas themselves are TileDocuments where the content is a [json-schema](https://json-schema.org){:target="\_blank"}. For example we can create a schema that requires a TileDocument to have a _title_ and a _message_.

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

First, use the `rewardSchema.commitId.toString()` to get the current [CommitID](../../learn/glossary.md#commitid) of the schema stream. When creating a TileDocument that uses this schema you need to use a CommitID instead of the StreamID. This is to enforce that we are using a specific version of the schema since the schema stream is mutable and can be updated.

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

Use `reward.state` to query the state of the TileDocument we just created. We can see that the schema is set to the correct CommitID.

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

Congratulations on completing this tutorial! You're well on your way to becoming a Ceramic developer. Now let's [install Ceramic in your project](./installation.md) or take a look at [IDX](../../tools/idx/overview.md) a framework for interacting with Streams.
