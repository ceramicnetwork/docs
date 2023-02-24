# **Ceramic Quickstart**

A simple introduction to Ceramic concepts. Learn the basics by using the Ceramic CLI.

## **Setup**
---

### **Prerequisites**
Ceramic requires [Node.js](https://nodejs.org/en/){:target="\_blank"} v16 and [npm](https://www.npmjs.com/get-npm){:target="\_blank"} v6.

!!! warning ""

    While npm v7 is not officially supported, you may still be able to get it to work. You will need to install the `node-pre-gyp` package globally. This is required until `node-webrtc` which IPFS depends on [is upgraded](https://github.com/node-webrtc/node-webrtc/pull/694){:target="_blank"}.

    ```bash
    npm install -g node-pre-gyp
    ```

### **Install Ceramic**

Open your console and install the CLIs using npm:

```sh
npm install --global @ceramicnetwork/cli @glazed/cli
```

- The Ceramic CLI will now be accessible as `ceramic` and the Glaze CLI as `glaze`.
- Run `ceramic help` and `glaze help` to list the available commands.

### **Launch Ceramic**

Start a local Ceramic node connected to the Clay Testnet at `https://localhost:7007`:

```sh
ceramic daemon
```

### **Create an account**

Ceramic transactions must be signed by an account ([DID](../../learn/glossary.md#dids)). The Glaze CLI can create a DID for you, after generating a 32-byte random seed:

```sh
glaze did:create
```

Your output should look something like this, with `...` used for brevity:

```sh
✔ Created DID did:key:z6Mk... with seed ab...f0
```

The seed can then be used when running other commands:

```sh
glaze [command] --key=ab...f0
DID_KEY=ab...f0 glaze [command]
```

**Note:** when entering --key, it should be your encoded seed, not the did:key itself.


## **Streams**
---

Ceramic usage depends on the particular types of streams you are interacting with, as they each have their own APIs. For this example the Glaze CLI interacts with tile documents, streams that store mutable JSON.

### **Create**

Create a new tile document using your account:

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
        The [StreamID](../../learn/glossary.md#streamid) is the unique ID of our stream. Your StreamID will be different since you created it with your account.

??? info "More options"

    - `--metadata`: set the *metadata* of the stream
    - Run `glaze tile:create -h` to see all available options

### **Query**

Load the _current state_ of a tile document using `tile:show` with a StreamID:

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

Load the _entire state_ of a stream using `stream:state` with a StreamID:

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

### **Update**

Use `tile:update` to update a tile document. Note that your [DID](../../learn/glossary.md#dids) must be assigned as the controller (author) of the stream to have permission to update it.

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


## **Schemas**
---

### **Creating a Schema**

Tile documents can enforce that their content adheres to a schema. The schemas themselves are actually also stored in tile documents where the content is [json-schema](https://json-schema.org/){:target="\_blank"}. 

Let's create a schema that requires a `title` and `message`:

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

### **Selecting a Version**

First, use the `stream:commits` command to list the commits contained in the schema stream. Find the specific commit (version) that you want to use.

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

### **Using the Schema**

When creating a tile document that uses this schema, we need to use the commitID instead of the StreamID to enforce that we are using a specific version of the schema, since the schema stream is mutable and can be updated. To create a stream that conforms to your schema, use the `tile:create` command and pass the `--metadata` option along with your commitID:

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


### **Confirmation**

Let's view the tile document we just created using the `stream:state` command from above. We will see that the schema is correctly set in the metadata.

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

## **Next Steps**
---
Congratulations on completing the quickstart tutorial! Visit [**Next Steps →**](../../docs/introduction/next-steps.md).
