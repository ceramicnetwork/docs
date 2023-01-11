# Quickstart

Learn the basics by setting up and interacting with the [Ceramic CLI](./installation.md). This tutorial serves as a simple introduction to Ceramic concepts.

!!! warning ""

    **Want an even faster way to try Ceramic?** Visit the [Playground demo app](https://playground.ceramic.dev){:target="_blank"} to test the full stack of Ceramic components in the browser.

## **1. Install the CLIs**

```sh
npm install --global @ceramicnetwork/cli @glazed/cli
```

The Ceramic CLI is then accessible as `ceramic` and the Glaze CLI as `glaze`.

Run `ceramic help` and `glaze help` to list the available commands.

## **2. Start the Ceramic daemon**

Run the `daemon` command of the Ceramic CLI to start a local Ceramic node:

```sh
ceramic daemon
```

Visit the [Ceramic CLI](./installation.md) page for more informations about the Ceramic daemon.

## **3. Create a Ceramic account**

Signing transactions to send on a Ceramic node requires a Ceramic account provided by a [DID](../../learn/glossary.md#dids).

The Glaze CLI can be used to create a key DID, generating a 32-byte random seed:

```sh
glaze did:create
```

The expected output will be similar to the following, with `...` used as placeholder for brevity:

```sh
✔ Created DID did:key:z6Mk... with seed ab...f0
```

The seed can then be used when running other commands:

```sh
glaze [command] --key=ab...f0
DID_KEY=ab...f0 glaze [command]
```

**Note:** when entering your --key here, it refers to the encoded seed generated for the private key, not the did:key itself.

## **4. Create a stream**

Use the `tile:create` command to create a new [stream](../../learn/glossary.md#streams). In the example below we create a stream that uses the [TileDocument StreamType](../../docs/advanced/standards/stream-programs/tile-document.md). Note that _TileDocument_ is the only [StreamType](../../learn/glossary.md#streamtypes) that can currently be created by the Ceramic CLI.

=== "Command"

    ```bash
    glaze tile:create --key ab...f0 --content '{"Foo":"Bar"}'
    ```

=== "Output"

    ```bash
    Created stream kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa.
    {
      streamID: 'kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa',
      content: { Foo: 'Bar' }
    }
    ```

    !!! quote ""
        Part pf the output is the [StreamID](../../learn/glossary.md#streamid), which is the persistent identifier of our newly created stream. This StreamID will be different for you, since you created it with your DID. Below the StreamID is the current content of the stream.

??? info "More options"

    - `--metadata`: set the *metadata* of the stream
    - Run `glaze tile:create -h` to see all available options

## **5. Query a stream**

Use the `tile:show` command to query the current [state](../../learn/glossary.md#state) of a stream. You will need to provide its _StreamID_.

=== "Command"

    ```bash
    glaze tile:show kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
    ```

    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output"

    ```bash
    Retrieved details of stream kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
    { Foo: 'Bar' }
    ```

Use the `stream:state` command to query the entire state of a stream.

=== "Command"

    ```bash
    glaze stream:state kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
    ```

    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output before anchor"

    ```bash
    {
      "type": 0,
      "content": {
        "Foo": "Bar"
      },
      "metadata": {
        "unique": "E4qPslUd0qo98TZX",
        "schema": null,
        "controllers": [
          "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
        ]
      },
      "signature": 2,
      "anchorStatus": "PENDING",
      "log": [
        {
          "cid": "bagcqceranhr345kxf5gb3kwjvvnnefn2krqvhkpnurphqrn3lj773mfszmza",
          "type": 0
        }
      ],
      "anchorScheduledFor": "1/24/2021, 11:45:00 AM"
    }
    ```

    !!! quote ""
        Here we can see various information about the stream such as *content*, *controllers*, and *schema*. In your output you should see your local DID as the controller, instead of the DID we show here. You will also see a different randomly-generated "unique" string for any TileDocument that was created without the `--deterministic` flag.  We can also see the current *anchorStatus* of our stream, and that it has been scheduled to be anchored at 11:45 on the 24th of January 2021. Once this anchor is finalized, the state of the stream will automatically be updated with a new entry in the log and *anchorStatus* will be set to `ANCHORED`.

=== "Output after anchor"

    ```bash
    {
      "type": 0,
      "content": {
        "Foo": "Bar"
      },
      "metadata": {
        "unique": "E4qPslUd0qo98TZX",
        "schema": null,
        "controllers": [
          "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
        ]
      },
      "signature": 2,
      "anchorStatus": "ANCHORED",
      "log": [
        {
          "cid": "bagcqceranhr345kxf5gb3kwjvvnnefn2krqvhkpnurphqrn3lj773mfszmza",
          "type": 0
        },
        {
          "cid": "bafyreig6hostufw42cmz2cnn7hpvb6pau67a2n2syhzej7orqxfymdayyq",
          "type": 2
        }
      ],
      "anchorScheduledFor": null,
      "anchorProof": {
        "root": "bagcqceranhr345kxf5gb3kwjvvnnefn2krqvhkpnurphqrn3lj773mfszmza",
        "txHash": "bagjqcgza4xgkpjodtqtgyu2fx6rdr6fb6mhevd5hy4253tl6pjlssidpwaha",
        "chainId": "eip155:3",
        "blockNumber": 9527752,
        "blockTimestamp": 1611485094
      }
    }
    ```

    !!! quote ""
        This output was seen after the anchor has been created. The stream state has now shifted *anchorStatus* to `ANCHORED`. You can also see that the log contains one more entry.

## **6. Update a stream**

Use the `tile:update` command to update a stream. Your [DID](../../learn/glossary.md#dids) must be the [controller](../../learn/glossary.md#controllers) of the stream in order to update it. Note that _TileDocument_ is the only StreamType that can currently be updated by the CLI.

=== "Command"

    ```bash
    glaze tile:update kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa --key ab...f0 --content '{"Foo":"Baz"}'
    ```

    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output"

    ```bash
    Updated stream
    {
      "streamID": "kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa",
      "content": {
        "Foo": "Baz"
      }
    }
    ```

??? info "More options"

    You can change _content_ and _metadata_ using the CLI. Run `glaze tile:update -h` for more information.

## **7. Create a schema**

TileDocuments can enforce that their contents adhere to a specified schema. The schemas themselves are TileDocuments where the content is a [json-schema](https://json-schema.org/){:target="\_blank"}. For example we can create a schema that requires a TileDocument to have a _title_ and _message_.

=== "Command"

    ```bash
    glaze tile:create --key ab...f0 --content '{
       "$schema": "http://json-schema.org/draft-07/schema#",
       "title": "Reward",
       "type": "object",
       "properties": {
         "title": {"type": "string"},
         "message": {"type": "string"}
       },
       "required": [
         "message",
         "title"
       ]
     }'
    ```

=== "Output"

    ```bash
    Created stream kjzl6cwe1jw1472as4pj3b3ahqmkokbmwc7jchqcob6pcixcoo4kxq6ls8uuxgb.
    {
      streamID: 'kjzl6cwe1jw1472as4pj3b3ahqmkokbmwc7jchqcob6pcixcoo4kxq6ls8uuxgb',
      content: {
        type: 'object',
        title: 'Reward',
        '$schema': 'http://json-schema.org/draft-07/schema#',
        required: [ 'message', 'title' ],
        properties: { title: { type: 'string' }, message: { type: 'string' } }
      }
    }
    ```

## **8. Create a TileDocument stream that uses a schema**

First, use the `stream:commits` command to list the [commitIDs](../../learn/glossary.md#commitid) contained in the schema stream. When creating a TileDocument that uses this schema, we need to use a commitID instead of the StreamID to enforce that we are using a specific version of the schema since the schema stream is mutable and can be updated.

=== "Command"

    ```bash
    glaze stream:commits kjzl6cwe1jw1472as4pj3b3ahqmkokbmwc7jchqcob6pcixcoo4kxq6ls8uuxgb
    ```

    !!! quote ""
        You should use your StreamID for the stream containing the json-schema you want to enforce, instead of the StreamID included here.

=== "Output"

    ```bash
    Stream commits loaded.
    [
      "k3y52l7qbv1frxu8co1hjrivem5cj2oiqtytlku3e4vjo92l67fkkvu6ywuzfxvy8"
    ]
    ```

If a stream contains multiple commits and you're not sure which one you want, use the `tile:show` command to show the content of the stream at the given commit.

Once you retrieve the desired commit, you can now create a TileDocument that is enforced to conform to this version of the schema. Use the `tile:create` command and pass the `--metadata` option along with your commitID.

=== "Command"

    ```bash
    glaze tile:create --key ab...f0 --content '{
        "title": "My first document with schema",
        "message": "Hello World"
      }' --metadata '{"schema":"k3y52l7qbv1frxu8co1hjrivem5cj2oiqtytlku3e4vjo92l67fkkvu6ywuzfxvy8"}'
    ```

    !!! quote ""
        You should use your commitID instead of the commitID included here.

=== "Output"

    ```bash
    Created stream kjzl6cwe1jw149tvfh6otqfzd2hfknkifb1z2lakozkicvau0xldzzdzwfbsztj.
    {
      streamID: 'kjzl6cwe1jw149tvfh6otqfzd2hfknkifb1z2lakozkicvau0xldzzdzwfbsztj',
      content: { title: 'My first document with schema', message: 'Hello World' }
    }
    ```

## **9. Query the stream you created**

Use the `stream:state` command to query the state of the TileDocument we just created. We can see that the schema is set to the correct commitID.

=== "Command"

    ```bash
    glaze stream:state kjzl6cwe1jw14b5sr79heovz7fziz4dxcn8upx3bcesriloqcui137k6rq6g2mn
    ```

    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output"

    ```bash
    Successfully queried stream kjzl6cwe1jw14b5sr79heovz7fziz4dxcn8upx3bcesriloqcui137k6rq6g2mn
    {
      "type": 0,
      "content": {
        "title": "My first document with schema",
        "message": "Hello World"
      },
      "metadata": {
        "unique": "GR5tBtHdaw608esV",
        "schema": "k3y52l7qbv1frxu8co1hjrivem5cj2oiqtytlku3e4vjo92l67fkkvu6ywuzfxvy8",
        "controllers": [
          "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
        ]
      },
      "signature": 2,
      "anchorStatus": "PENDING",
      "log": [
        {
          "cid": "bagcqcera5qxg5zabjjvwpcbia6c3t6vebgo4brgmsagxezdjgk4vxnzwb5hq",
          "type": 0
        }
      ],
      "anchorScheduledFor": "1/13/2021, 1:45:00 PM"
    }
    ```

# **That's it!**

Congratulations on completing this tutorial! You're well on your way to becoming a Ceramic developer. Now let's [install Ceramic in your project →](../javascript/installation.md)
