# **Streams**

---

Streams are individual instances of state on the Ceramic network. Every stream is mutable and can only be modified when it receives a transaction signed by the account that owns it.

## **How streams work**

---

### **Stream identifiers**

Every stream is identified by its `streamID`, which is its unique address on the Ceramic network. When applications load a stream from the network using its streamID, its current state is returned. Alternative, applications can load a previous version of a stream's state by using the `commitID` of the specific version wanted.

### **Streamcode**

Every stream that is created on Ceramic must reference a `streamcode` in its metadata, which is a script that contains the processing logic used to transform a stream's current state into the next state upon receipt of a new transaction. In general, you can think of streamcode as reusable state processing logic and streams as the individual states it generates.

Today Ceramic supports two types of streams: tile documents which store mutable JSON documents with schema validation, and CAIP-10 links which store a link between a Web3 wallet account and a Ceramic account.

### **Metadata**

Every stream can specify a few metadata properties:

| Metadata property | Required? | Description                                          |
| ----------------- | --------- | ---------------------------------------------------- |
| `streamtype`      | Yes       | The streamcode used by the stream                    |
| `controller`      | Yes       | The Ceramic account (DID) that can modify the stream |
| `schema`          | No        | The schema for the stream                            |
| `family`          | No        | Used to associate collections of streams             |
| `tags`            | No        | Used to create subgroupings of streams               |

### **Content**

The type of content that a stream can store is determined by its streamcode.

## **Building with streams**

---

### [**Store JSON content →**](tile-document.md)

[Tile Document (CIP-8)](tile-document.md) is streamcode that stores a mutable JSON document with schema validation, providing similar functionality as a NoSQL document.

### [**Store blockchain account links →**](caip10-link.md)

[CAIP-10 Link (CIP-7)](caip10-link.md) is streamcode that stores a link between a Web3 wallet account and a Ceramic account.

### **New streamcode**

Ceramic does not yet suport the arbitrary creation of new streamcode. If you'd like to create new streamcode to support additional use cases for streams, reach out on the [Ceramic Discord](https://chat.ceramic.network).
