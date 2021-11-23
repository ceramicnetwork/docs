---
hide:
  - toc
---

# Getting started with Ceramic

In this tutorial you will build a hello world microblogging application on Ceramic in under 5 minutes without leaving the page. You do not need to deploy or install Ceramic to complete this tutorial. In the process of creating your application, you will learn Ceramic fundamentals including storing and updating data on the network using transactions, indexing data in an account-centric way, and querying data from the network. 

## Things to know

- Ceramic stores data in [streams](), which are individual states whose mutations are controlled by executable programs. 
- Users can update the streams they own by sending them transactions. If a stream does not exist, Ceramic creates the stream when you create the first transaction.
- The following example uses the [CIP-7: Tile]() stream program to store data. Tile are a widely-adopted community standard for storing data in mutable JSON objects, similar to a NoSQL document.
- In this tutorial we will build a basic microblogging application.
- The examples in this tutorial use a subset of the [Sample Mflix Dataset](https://docs.atlas.mongodb.com/sample-data/sample-mflix/), which is part of the sample data stored on Ceramic's [Clay Testnet](). Atlas requires no installation overhead and offers a free tier to get started. After completing this tutorial, you can use Ceramic to explore additional sample data or host your own data.

---

```js
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
```

**Click inside the shell to connect.** *Behind the scenes, the shell creates a throwaway account for you which will be used to sign your transactions. Once connected, you can run the examples in the shell. Copy and paste the examples into the shell above.*

## Step 1: Deploy data models

---

The first part of building any Ceramic application is deciding what you want to store and deploying schemas for your data. For this tutorial, our microblogging application will use two schemas: one for a post and another for a list of posts.

Use the `stream.tileDocument.create()` method to create a new Tile that stores a JSON schema for a post:

``` ts
.
.
.
.
.
.
.
.
.
.
.
.
.
console.log streamID
```

Use the same method to create another Tile to store the JSON schema for a list of posts:

``` ts
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
.
console.log streamID
```

## Step 2: Store and update data

---

Next, let's start populating Ceramic with some microblogging data. Typically this would happen during runtime as a user is interacting with your application.

Publish your first micro-blog post and then add it to your list of posts:

``` ts
.
create post
.
.
.
.
.
create list of posts and add your post.
.
.
.
.
.
```

Publish your second micro-blog post and then add it to your list of posts:

``` ts
.
create post
.
.
.
.
.
update list of posts and add your post.
.
.
.
.
.
```

Now, use the `stream.tileDocument.update()` method to modify one of your posts:

``` ts
.
.
.
.
.
.
.
```

## Step 3: Retrieve data

---

Next, let's retrieve your data from the network. First, let's use the `stream.tileDocument.load()` load your list of posts:

``` ts
.
.
.
.
.
.
.
```

Now, call `ceramic.multiQuery()` method to load your posts:

``` ts
.
.
.
.
.
.
.
```

Now, call `ceramic.loadStream.atCommit()` method to load the historical version of the post that you updated:

``` ts
.
.
.
.
.
.
.
```

## Advanced

---

### Step 4: Index data

Because you must know a stream's identifier (streamID) in order to retrieve it from the network, the basic retrieval flow above only really works for a siloed application within a single session. This is because the identifiers of the streams belonging to a given user can be temporarily stored client-side in your application state. But what happens to those stream IDs after a user signs out or if they want to use those streams within another application?

If you're building an application that aims to support a continuous user experience across multiple sessions or if you want to make the data created within your application available for use within other applications (i.e. interoperability or composability), you will need a decentralized indexing strategy for a user's streams. For this, we will use the [CIP-11: Account-based indexing (IDX)]() standard, which is a widely-adopted community standard for account-centric indexes.

To enable account-centric indexing with IDX, we'l need to add some new features to our application. Since we really only need to know the streamID of the user's list of posts in order to retrieve their posts, we'll want to add the streamID of their list of posts to their index. 

To do this, we'll first need to create an IDX definition for our post list. A definition is a new stream that extends a schema with additional metadata and functions as a key in the user's index. It also provides a layer of abstraction between the schema and the index which allows the schema to evolve and change over time without needing a new entry in the index.

``` ts
.
.
. // Create the IDX definition
.
. // Add the IDX definition with some note about needing to add their schema streamID
.
.
.
.
.
.
```

Next, we'll need to modify the metadata of our post list to include the identifier of our IDX definition. This allows the network observers to know that all post lists stored on the network (of all users) belong to the same family. If you had set up your application with indexing from the beginning this could have been done when you initially created the stream.

``` ts
ceramic.stream.tileDocument.update ()
.
. // add metadata for idx definition
.
.
.
.
.
.
.
```

Now, we need to a create a new stream for the user

### Step 5: Query data using the index



   

    ``` ts
    await ceramic.stream.tileDocument.create( { "Foo": "Bar" }'
    
    // this example should be of a basic profile schema
    
    
    )

    console.log
    ```

    Next, we'll create another new tile which stores a social media post.

    ### Update a stream

    Then, update your stream:

    ``` ts
    await ceramic.stream.tileDocument.create( { "Foo": "Bar" }'
    
    // this example should be of a basic profile
    
    
    )
    ```

=== "1. Check network"

    ### Check network

    ```bash
    ceramic create tile --content '{ "Foo": "Bar" }'
    ```


=== "2. Perform transactions"

    ```bash
    StreamID(kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa)
    {
        "Foo": "Bar"
    }
    ```

    !!! quote ""
        The first line of the output is the [StreamID](../../learn/glossary.md#streamid), which is the persistent identifier of our newly created stream. This StreamID will be different for you, since you created it with your DID. Below the StreamID is the current content of the stream.

=== "3. Load Data"

    ```bash
    StreamID(kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa)
    {
        "Foo": "Bar"
    }
    ```

    !!! quote ""
        The first line of the output is the [StreamID](../../learn/glossary.md#streamid), which is the persistent identifier of our newly created stream. This StreamID will be different for you, since you created it with your DID. Below the StreamID is the current content of the stream.

??? info "More options"

    - `--controllers`: set the *controller* of the stream
    - `--schema`: set the *schema* of the TileDocument
    - Run `ceramic create -h` to see all available options

## Improve your application

This application is just a hello world. What makes it such? These are all the things you would improve if you were building the real thing...

- Thing 1
- Thing 2
- Thing 3
- Thing 4


## Next Steps

### Set Up Your Own Deployment

| Syntax      | Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |

### Additional Examples
