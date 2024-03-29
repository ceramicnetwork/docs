# **URI Scheme**

---

## **Stream URL**

---

Each stream in Ceramic is identified by a unique URL. This URL is comprised of a protocol identifier for Ceramic and a StreamId as defined below.

When encoded as a string the StreamID is prepended with the protocol handler and StreamID is typically encoded using `base36`. This fully describes which stream and where it is located, in this case it can be found on the Ceramic Network.


```html
ceramic://<StreamId>
```

For example, a StreamId may look as follows:

```html
ceramic://kjzl6fddub9hxf2q312a5qjt9ra3oyzb7lthsrtwhne0wu54iuvj852bw9wxfvs
```

EventIds can also be encoded in the same way. 

```html
ceramic://<EventId>
```

## **StreamId**

---

A StreamId is composed of a StreamId code, a stream type, and a CID. It is used to reference a specific and unique event stream. StreamIds are similar to CIDs in IPLD, and use multiformats, but they provide additional information specific to Ceramic event streams. This also allows them to be distinguished from CIDs. The *init event* of an event stream is used to create the StreamId. 

StreamIds are defined as:

```html
<streamid> ::= <multibase-prefix><multicodec-streamid><stream-type><init-cid-bytes>

# e.g. using CIDv1
<streamid> ::= <multibase-prefix><multicodec-streamid><stream-type><multicodec-cidv1><multicodec-content-type><multihash-content-address>
```

Where:

- **`<multibase-prefix>`** is a [multibase](https://github.com/multiformats/multibase) code (1 or 2 bytes), to ease encoding StreamIds into various bases. **NOTE:** Binary (not text-based) protocols and formats may omit the multibase prefix when the encoding is unambiguous.
- **`<multicodec-streamid>`** `0xce` is a [multicodec](https://github.com/multiformats/multicodec) used to indicate that it's a [StreamId](https://github.com/multiformats/multicodec/blob/master/table.csv#L78), encoded as a varint
- **`<stream-type>`** is a [varint](https://github.com/multiformats/unsigned-varint) representing the stream type of the stream.
- **`<init-cid-bytes>`** is the bytes from the [CID](https://github.com/multiformats/cid) of the `init event`,  stripped of the multibase prefix.

The multicodec for StreamID is [`0xce`](https://github.com/multiformats/multicodec/blob/master/table.csv#L78). For compatibility with browser urls it's recommended to encode the StreamId using [[`base36`]](https://github.com/multiformats/multibase).

The stream type value does not currently have any functionality at the protocol level. Rather, it is used by applications building on top of Ceramic (e.g. ComposeDB) to distinguish between different logic that is applied when processing events. Stream Type values have to be registered in the table of [CIP-59](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-59/CIP-59.md#registered-values). 

## **EventId**

---

EventIds extend StreamIds to reference a specific event in a specific stream. Additional bytes are added to the end of a StreamId. If it represents the genesis event the zero byte is added (`0x00`) otherwise the CID that represents the event is added.

EventIds are defined as

```html
<streamid> ::= <multibase-prefix><multicodec-streamid><stream-type>
  <genesis-cid-bytes>
  <event-reference>

# e.g. using CIDv1 and representing the genesis event
<streamid> ::= <multibase-prefix><multicodec-streamid><stream-type>
  <multicodec-cidv1><multicodec-content-type><multihash-content-address>
  <0x00>

# e.g. using CIDv1 and representing an arbitrary event in the log
<streamid> ::= <multibase-prefix><multicodec-streamid><stream-type>
  <multicodec-cidv1><multicodec-content-type-1><multihash-content-address-1>
  <multicodec-cidv1><multicodec-content-type-2><multihash-content-address-2>

```

Where:

- **`<event-reference>`** is either the zero byte (`0x00`) or [CID](https://github.com/multiformats/cid) bytes.

### **Stream Versions**

Each EventId can also be considered a reference to a specific version of a stream. At any EventId, a stream can be loaded up until that event and the resulting set of events are considered the version of that stream.
