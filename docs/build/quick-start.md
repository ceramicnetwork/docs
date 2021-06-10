# Quick start
Learn the basics by setting up and interacting with the Ceramic CLI. This tutorial serves as a simple introduction to Ceramic concepts. See [installation](./installation.md) to install Ceramic in your project and start building applications.

> **Want an even faster way to try Ceramic?** Visit the [Playground demo app](https://playground.ceramic.dev){:target="_blank"} to test the full stack of Ceramic components in the browser without needing to install anything.

## **Prerequisites**

This quick start guide will use a terminal, [Node.js](https://nodejs.org/en/){:target="_blank"} v14, and [npm](https://www.npmjs.com/get-npm){:target="_blank"} v6. Make sure to have these installed on your machine.

While npm v7 is not officially supported, you may be able to get it to work anyway, however you will need to have the `node-pre-gyp` package installed globally:
```bash
npm install -g node-pre-gyp
```
This is required until node-webrtc (which IPFS depends on) [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694).

## **Install the CLI**

Install the Ceramic CLI using your terminal.

```bash
npm install -g @ceramicnetwork/cli
```

## **Start the daemon**

Start a Ceramic daemon on your local machine and automatically connect to it on
port 7007, `http://localhost:7007`.

```bash
ceramic daemon
```

??? info "Node configurations"
    There are multiple options you can configure when you start the ceramic daemon.
    
    - **Network**: By default the CLI starts a Ceramic node on the `clay` testnet. If you
    would like to use a different Ceramic network, you can specify this with the
    `--network` option.
    - **Additional configurations**: Use the `ceramic daemon -h` command to see additional options.


## **Authentication**
By default, the Ceramic CLI is authenticated with a
[Key DID](https://github.com/ceramicnetwork/key-did-provider-ed25519){:target="_blank"}. The seed
for this DID is stored in `~/.ceramic/config.json`. If this file is not present
on startup a new DID will be randomly generated. It's currently not possible to
use the Ceramic CLI with other DID methods.


## **Create a stream**
Use the `create` command to create a new stream. In the example below
we create a stream of type *TileDocument*. Note that *TileDocument* is
the only stream type that can currently be created by the Ceramic CLI.

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
        The first line of the output is the *StreamID*, which is the persistent identifier of our newly created stream. This StreamID will be different for you, since you created it with your DID. Below the StreamID is the current content of the stream.

??? info "More options"
    
    - `--controllers`: set the *controller* of the stream
    - `--schema`: set the *schema* of the TileDocument
    - Run `ceramic create -h` to see all available options

## **Query a stream**
Use the `show` command to query the current state of a stream. You will need to provide its *StreamID*.

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

## **Update a stream**
Use the `update` command to update a stream. Your DID must be the controller of the stream in order to update it.
Note that *TileDocument* is the only stream type that can currently be updated by the CLI.

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


## **Create a schema**
Ceramic TileDocuments can enforce that their contents adhere to a specified schema. The schemas themselves are Ceramic TileDocuments where the content is a [json-schema](https://json-schema.org/){:target="_blank"}. For example we can create a schema that requires a document to have a *title* and *message*.

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

## **Create a TileDocument that uses a schema**
First, use the `commits` command to list the commitIDs contained in the schema document. When creating a TileDocument that uses this schema, we need to use a commitID instead of the DocID to enforce that we are using a specific version of the schema since the schema document is mutable and can be updated.

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

## **Query the document you created**
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
Congratulations on completing this tutorial! You're well on your way to becoming a Ceramic developer. Now let's [install Ceramic in your project →](./installation.md)
</br>
</br>
</br>
