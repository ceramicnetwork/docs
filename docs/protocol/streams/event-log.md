# Event Log

---

The core data structure in the Ceramic protocol is a self-certifying event log. It combines IPLD for hash linked data and cryptographic proofs to create an authenticated and immutable log. This event log can be used to model mutable databases and other data structures on top.

## Introduction

---

Append-only logs are frequently used as an underlying immutable data structure in distributed systems to improve data integrity, consistency, performance, history, etc. Open distributed systems use hash linked lists/logs to allow others to verify the integrity of any data. IPLD provides a natural way to define an immutable append-only log. 

- **Web3 authentication** - When combined with cryptographic signatures and blockchain timestamping, it allows authenticated writes to these logs using blockchain accounts and DIDs
- **Low cost decentralization** - Providing a common database layer for users and applications besides more expensive on-chain data or centralized and siloed databases
- **Interoperability, flexibility, composability** - A minimally defined log structure allows a base level of interoperability while allowing diverse implementations of mutable databases and data structures on top. Base levels of interoperability include log transport, update syncing, consensus, etc.

## Events

---

Logs are made up of events. An init event originates a new log and is used to reference or name a log. The name of a stream is referred to as a [StreamId](uri-scheme.md#streamid). Every additional "update" is appended as a data event. Periodically, time events are added after one or more data events. Time events allow you to prove an event was published at or before some point in time using blockchain timestamping. They can also be used for ordering events within streams and for global ordering across streams and blockchain events. The minimal definition of a log is provided here, additional parameters in both the headers and body are defined at application level or by higher level protocols. 

Data events (and often Init Events) are signed DAGJWS and encoded in IPLD using the [DAG-JOSE codec](https://ipld.io/specs/codecs/dag-jose/spec/). Event payloads are typically encoded as DAG-CBOR, but could be encoded with any codec supported by a node or the network. Formats and types are described using [IPLD schema language](https://ipld.io/docs/schemas/) and event encoding is further described below. 

### Init Event

A log is initialized with an init event. The CID of this event is used to reference or name this log in a [StreamId](uri-scheme.md#streamid). An init event may be signed or unsigned.

```tsx

type InitHeader struct {
  controllers [String]
}
type InitPayload struct {
  header InitHeader
  data optional Any 
}

type InitJWS struct { // This is a DagJWS
  payload String
  signatures [Signature]
  link: &InitPayload
}

type InitEvent InitPayload | InitJWS

```

**Parameters defined as follows:**

- **`controllers`** - an array of DID strings that defines which DIDs can write events to the log, when using CACAO, the DID is expected to be the issuer of the CACAO. Note that currently only a single DID is supported.
- **`data`** - data is anything, if defined the Init Event must match the InitJWS struct or envelope and be encoded in DAG-JOSE, otherwise the InitPayload would be a valid init event alone and encoded in DAG-CBOR

### Data Event

Log updates are data events. Data events are appended in the log to an init event, prior data events or a time event. A data event MUST be signed. 

```tsx
type Event InitEvent | DataEvent | TimeEvent

type DataHeader struct {
  controllers optional [String]
}

type DataEventPayload struct {
  id &InitEvent
  prev &Event
  header optional DataHeader
  data Any 
}

type DataEvent struct { // This is a DagJWS
  payload String
  signatures [Signature]
  link: &DataEventPayload
}
```

Additional parameters defined as follows, controllers and data defined same as above.

- **`id`** - CID (Link) to the init event of the log
- **`prev`** - CID (Link) to prior event in log
- **`header`** - Optional header, included here only if changing header parameter value (controllers) from prior event. Other header values may be included outside this specification.

This being a minimally defined log on IPLD, later specifications or protocols can add additional parameters to both init and data events and their headers as needed. 

### Time Event

Time events can be appended to init events, and 1 or more data events. Reference [CAIP-168 IPLD Timestamp Proof](https://chainagnostic.org/CAIPs/caip-168) specification for more details on creating and verifying time events. Time Events are a simple extension of the IPLD Timestamp Proof specification, where `prev` points to the prior event in the log and is expected to be the data for which the timestamp proof is for. A timestamp event is unsigned.

```tsx
type TimeEvent struct {
  id &InitEvent
  prev &DataEvent | &InitEvent
  proof Link
  path String
}
```

## Verification

---

A valid log is one that includes data events as defined above and traversing the log resolves to an originating init event as defined above. Each event is valid when it includes the required parameters above and the DAGJWS signature is valid for the given event `controller` DID and valid as defined below. Time events are defined as valid by CAIP-168. There will likely be additional verification steps specific to any protocol or application level definition.

## Encoding

---

### JWS & DAG-JOSE

All signed events are encoded in IPLD using [DAG-JOSE](https://ipld.io/specs/codecs/dag-jose/spec/). DAG-JOSE is a codec and standard for encoding JOSE objects in IPLD. JOSE includes both[JWS](https://datatracker.ietf.org/doc/rfc7515/?include_text=1) for signed JSON objects and [JWE](https://datatracker.ietf.org/doc/rfc7516/?include_text=1) for encrypted JSON objects. JWS is used for events here and is commonly used standard for signed data payloads. Some parameters are further defined for streams. The original DAG-JOSE specification can be found [here](https://ipld.io/specs/codecs/dag-jose/spec/).

The following defines a general signed event, both init and data events are more specifically defined above. 

```tsx
type Signature struct {
  header optional { String : Any }
  // The base64url encoded protected header, contains:
  // `kid` - the DID URL used to sign the JWS
  // `cap` - IPFS url of the CACAO used (optional)
  protected optional String
  signature String
}

type EventJWS struct {
  payload String
  signatures [Signature]
  link: &Event
}
```

Where:

- **`link`** - CID (Link) to the event for which this signature is over. Provided for easy application access and IPLD traversal, expected to match CID encoded in payload
- **`payload`** - base64url encoded CID link to the event (JWS payload) for which this signature is over
- **`protected`** - base64 encoded JWS protected header
- **`header`** - base64 encoded JWS header
- **`signature`** - base64 encoded JWS signature