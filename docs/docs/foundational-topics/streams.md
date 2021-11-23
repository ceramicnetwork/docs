# Streams

A stream is a data object (state) stored on the Ceramic network. It resides at a specific address, called a streamID. Accounts designated as the controller of the stream are able to modify its state by sending it transactions.

## State

---

Here's an example of a stream:

```
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

### Stream Metadata

You'll notice the stream includes the folowing metadata fields:

| Field | Required | Description | 
| --- | --- | --- |
| `streamType` | Y | The program used by this stream. |
| `controllers` | Y | Accounts allowed to send transactions. |
| `schema` | N | JSON schema used for the content. |
| `family` | N | Arbitrary string. |
| `tags` | N | Array of arbitrary strings. |


### Stream Content

Also, and perhaps most importantly, a stream contains of a content field, which can be used to store arbitrary content.

## Program

---

## Controllers

---

## Schema

---


## The Stream Chain

---

The underlying data structure. You can think of ... a chain of linked transactions. history and makes the stream syncable over IPFS.

## Further Reading

---

## Related Topics

---

- Thing 1
- Thing 2

