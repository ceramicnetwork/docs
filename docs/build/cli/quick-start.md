# Quickstart
Learn the basics by setting up and interacting with the [Ceramic CLI](./javascript/installation.md). This tutorial serves as a simple introduction to Ceramic concepts.

!!! warning ""

    **Want an even faster way to try Ceramic?** Visit the [Playground demo app](https://playground.ceramic.dev){:target="_blank"} to test the full stack of Ceramic components in the browser.

## **1. Install the CLI**

Visit the [Ceramic CLI](./cli/installation.md) page for instructions on how to quickly install the CLI.

## **2. Create a stream**
Use the `create` command to create a new [stream](../learn/glossary.md#streams). In the example below we create a stream that uses the [TileDocument StreamType](../streamtypes/tile-document/overview.md). Note that *TileDocument* is the only [StreamType](../learn/glossary.md#streamtypes) that can currently be created by the Ceramic CLI.

=== "Command"

    ```bash
    $ ceramic create tile --content '{ "Foo": "Bar" }'
    ```

=== "Output"

    ```bash
    StreamID(kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa)
    {
        "Foo": "Bar"
    }
    ```

    !!! quote ""
        The first line of the output is the [StreamID](../learn/glossary.md#streamid), which is the persistent identifier of our newly created stream. This StreamID will be different for you, since you created it with your DID. Below the StreamID is the current content of the stream.

??? info "More options"
    
    - `--controllers`: set the *controller* of the stream
    - `--schema`: set the *schema* of the TileDocument
    - Run `ceramic create -h` to see all available options

## **3. Query a stream**
Use the `show` command to query the current [state](../learn/glossary.md#state) of a stream. You will need to provide its *StreamID*.

=== "Command"

    ```bash
    $ ceramic show kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
    ```
    
    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output"

    ```bash
    {
        "Foo": "Bar"
    }
    ```


Use the `state` command to query the entire state of a stream.

=== "Command"

    ```bash
    $ ceramic state kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
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

## **4. Update a stream**
Use the `update` command to update a stream. Your [DID](../learn/glossary.md#dids) must be the [controller](../learn/glossary.md#controllers) of the stream in order to update it. Note that *TileDocument* is the only StreamType that can currently be updated by the CLI.

=== "Command"

    ```bash
    $ ceramic update kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa --content '{
        "Foo": "Baz"
      }'
    ```
    
    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output"

    ```bash
    {
      "Foo": "Baz"
    }
    ```

??? info "More options"
    Currently you can change *content*, *controllers*, and *schema* using the CLI. Run `ceramic update -h` for more information.


## **5. Create a schema**
TileDocuments can enforce that their contents adhere to a specified schema. The schemas themselves are TileDocuments where the content is a [json-schema](https://json-schema.org/){:target="_blank"}. For example we can create a schema that requires a TileDocument to have a *title* and *message*.

=== "Command"

    ```bash
    $ ceramic create tile --content ' {
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
     }'
    ```

=== "Output"

    ```bash
    StreamID(kjzl6cwe1jw1472as4pj3b3ahqmkokbmwc7jchqcob6pcixcoo4kxq6ls8uuxgb)
    {
      "type": "object",
      "title": "Reward",
      "$schema": "http://json-schema.org/draft-07/schema#",
      "required": [
        "message",
        "title"
      ],
      "properties": {
        "title": {
          "type": "string"
        },
        "message": {
          "type": "string"
        }
      }
    }
    ```

## **6. Create a TileDocument stream that uses a schema**
First, use the `commits` command to list the [commitIDs](../learn/glossary.md#commitid) contained in the schema stream. When creating a TileDocument that uses this schema, we need to use a commitID instead of the StreamID to enforce that we are using a specific version of the schema since the schema stream is mutable and can be updated.

=== "Command"

    ```bash
    $ ceramic commits kjzl6cwe1jw1472as4pj3b3ahqmkokbmwc7jchqcob6pcixcoo4kxq6ls8uuxgb
    ```
    
    !!! quote ""
        You should use your StreamID for the stream containing the json-schema you want to enforce, instead of the StreamID included here.

=== "Output"

    ```bash
    [
      "k3y52l7qbv1frxu8co1hjrivem5cj2oiqtytlku3e4vjo92l67fkkvu6ywuzfxvy8"
    ]
    ```


If a stream contains multiple commits and you're not sure which one you want, use the `show` command to show the content of the stream at the given commit.

Once you retrieve the desired commit, you can now create a TileDocument that is enforced to conform to this version of the schema. Use the `create` command and pass the `--schema` option along with your commitID.

=== "Command"

    ```bash
    $ ceramic create tile --content '{
        "title": "My first document with schema",
        "message": "Hello World"
      }' --schema k3y52l7qbv1frxu8co1hjrivem5cj2oiqtytlku3e4vjo92l67fkkvu6ywuzfxvy8
    ```
    
    !!! quote ""
        You should use your commitID instead of the commitID included here.

=== "Output"

    ```bash
    StreamID(kjzl6cwe1jw149tvfh6otqfzd2hfknkifb1z2lakozkicvau0xldzzdzwfbsztj)
    {
      "title": "My first document with schema",
      "message": "Hello World"
    }
    ```

## **7. Query the stream you created**
Use the `state` command to query the state of the TileDocument we just created. We can see that the schema is set to the correct commitID.

=== "Command"

    ```bash
    $ ceramic state kjzl6cwe1jw14b5sr79heovz7fziz4dxcn8upx3bcesriloqcui137k6rq6g2mn
    ```
    
    !!! quote ""
        You should use your StreamID instead of the StreamID included here.

=== "Output"

    ```bash
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
Congratulations on completing this tutorial! You're well on your way to becoming a Ceramic developer. Now let's [install Ceramic in your project →](./javascript/installation.md)
</br>
</br>
</br>
