# **The Ceramic stack**

---

The Ceramic application development stack consists of a few key components which are listed below, in order from highest-level frameworks to lowest-level network APIs.

## **1. Frameworks**

---

Frameworks abstract away much of the complexity and configuration of the underlying stack, providing developers with a simple entrypoint into Ceramic development. In order to achieve simplicity, frameworks tend to be opinionated about _how_ and _what_ you build on the network.

> The most popular framework for Ceramic is the [Self.ID SDK](../reference/self-id/index.md), a bundle that includes everything you need to build applications with composable, user-centric Web3 data.

## **2. Middleware**

---

Middleware is a generic name for all kinds of development tools that are not a part of the core Ceramic protocol, but provide developers with additional functionality and convenience.

> The most popular middleware for Ceramic is [Glaze suite](../reference/glaze/index.md), which provides a set of libraries that developers can use alongside a Ceramic client when building applications.

## **3. Data models**

---

Data models are collections of one or more streams, specified by their schemas and relationships, that define a single data structure on Ceramic. Data models form the basis of composability on Ceramic. When multiple applications reuse the same data model, they can reuse the same data. Typically, data models are used to represent an application feature such as a social graph or a user profile.

> The most popular data models for Ceramic can be found in the [Data Models Registry →](../docs/advanced/standards/data-models/data-model-universe.md), an open registry of Ceramic data models created by the community.

## **4. Streams**

---

Streams are individual instances of state on the Ceramic network. Every stream that is created on Ceramic must specify its streamcode, which is a script that contains the processing logic used to transform a stream's current state into the next state upon receipt of a new transaction. In general, you can think of streamcode as reusable state processing logic and streams as the individual states it generates.

> Today Ceramic supports two types of streams: [tile documents](../docs/advanced/standards/stream-programs/tile-document.md) which store mutable JSON documents with schema validation, and [CAIP-10 links](../docs/advanced/standards/stream-programs/caip10-link.md) which store a link between a Web3 wallet account and a Ceramic account.

## **5. Accounts**

---

Accounts are user entities on Ceramic that can own streams and submit transactions to those streams. Ceramic accounts conform to the standard decentralized identifier (DID) specification, as outlined by the Decentralized Identity Foundation (DIF). DIDs are useful as they serve to unbundle Ceramic accounts from any single Web3 wallet address or public key, allowing users to control the same Ceramic account from one or more Web3 wallet accounts, and in the process providing an abstraction to enable truly cross-chain accounts.

> The most popular account client is the [DID JSON-RPC client](../reference/core-clients/did-jsonrpc.md), which provides a standard account API and must be used alongside a Ceramic client in order to enable authenticated accounts to perform transactions on the network. The DID client currently supports three different account types: [PKH DID](../docs/advanced/standards/accounts/pkh-did.md), will allows you to use a Web3 wallet adress as a Ceramic account, [3ID DID](../docs/advanced/standards/accounts/3id-did.md), which allows controlling a Ceramic account from multiple Web3 wallet addresses, and [Key DID](../docs/advanced/standards/accounts/key-did.md), which can only be controlled by a single key.

## **6. Clients**

---

Clients libraries allow your application to connect to a Ceramic node. Different clients may choose to implement different high-level, language-specific developer APIs. Before submitting requests to a Ceramic node, clients translate those API calls into the standard Ceramic HTTP API, which it uses to actually communicate with a Ceramic node.

> The most popular client on Ceramic is the [JS HTTP client](../reference/core-clients/ceramic-http.md), allowing developers to connect their application to Ceramic using JavaScript.

## **7. Network APIs**

---

The Ceramic HTTP API is the lowest-level interface for Ceramic. It is used under the hood by every client and node to communicate. Unless application developers have a specific need to use HTTP, most never need to interact with this API directly, instead accessing it via more developer-friendly client APIs. Protocol developers wishing to write a client in a new language, however, would need to use this specification.

> Learn more about the [Ceramic HTTP API](../build/http/api.md).
