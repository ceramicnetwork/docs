# CLI API

This page describes CLI commands for interacting with Ceramic.

!!! warning "Depreciation Notice:"

    Interactions with Streams and TileDocuments have been moved to our [Glaze CLI](../../../tools/glaze/development#cli). If you encounter any errors with the commands below please retry using the Glaze CLI first.

## **Requirements**

You should have already [installed the CLI](./installation.md).

## **Write APIs**

### **Create a stream**

Use the `create` command to create a new [stream](../../learn/glossary.md#streams). In the example below we create a stream that uses the [TileDocument StreamType](../../docs/advanced/standards/stream-programs/tile-document.md). Note that _TileDocument_ is the only [StreamType](../../learn/glossary.md#streamtypes) that can currently be created by the Ceramic CLI.

```bash
ceramic create tile --content '{ "Foo": "Bar" }'
```

The first line of your output will be the [StreamID](../../learn/glossary.md#streamid) of your stream. Below the StreamID, you will see it's content.

**Create options:**

Run `ceramic create -h` to see all available create options. Some common options:

- `--content`: set the _content_ of the stream
- `--controllers`: set the _controller_ of the stream
- `--schema`: set the _schema_ of the TileDocument

### **Update a stream**

Use the `update` command to update a stream. You will need to provide a _StreamID_. Your [DID](../../learn/glossary.md#dids) must be the [controller](../../learn/glossary.md#controllers) of the stream in order to update it. Note that _TileDocument_ is the only StreamType that can currently be updated by the CLI.

```bash
ceramic update kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa --content '{
    "Foo": "Baz"
  }'
```

**Update options:**

Run `ceramic update -h` to see all available update options. Some common options:

- `--content`: update the _content_ of the stream
- `--controllers`: update the _controller_ of the stream
- `--schema`: update the _schema_ of the TileDocument

## **Query APIs**

### **Query a stream's current state**

Use the `show` command to query the current [state](../../learn/glossary.md#state) of a stream. You will need to provide a _StreamID_.

```bash
ceramic show kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
```

### **Query a stream's entire state**

Use the `state` command to query the entire state of a stream. You will need to provide a _StreamID_.

```bash
ceramic state kjzl6cwe1jw147ww5d8pswh1hjh686mut8v1br10dar8l9a3n1wf8z38l0bg8qa
```

## **Next steps**

If you haven't already, try putting these CLI commands to use in the [Quick Start](quick-start.md) guide.
