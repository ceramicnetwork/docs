# Accounts JSON-RPC

In order for a software application to interact with the Ceramic network (by reading data and/or sending transactions to the network), it must connect to a Ceramic node.

For this purpose, every Ceramic client implements an HTTP API specification, so there are a uniform set of methods that applications can rely on.

JSON-RPC is a stateless, light-weight remote procedure call (RPC) protocol. Primarily the specification defines several data structures and the rules around their processing. It is transport agnostic in that the concepts can be used within the same process, over sockets, over HTTP, or in many various message passing environments. It uses JSON (RFC 4627) as data format.

## JSON-RPC Resources

---

- [Ethereum JSON-RPC Specification ↗]()
- [Ethereum JSON-RPC Specification GitHub repo ↗]()

## Client implementations

---

Ethereum clients each may utilize different programming languages when implementing the JSON-RPC specification. See individual client documentation for further details related to specific programming languages. We recommend checking the documentation of each client for the latest API support information.

## Convenience libraries

---

While you may choose to interact directly with Ethereum clients via the JSON-RPC API, there are often easier options for dapp developers. Many JavaScript and backend API libraries exist to provide wrappers on top of the JSON-RPC API. With these libraries, developers can write intuitive, one-line methods in the programming language of their choice to initialize JSON-RPC requests (under the hood) that interact with Ethereum.

## Related topics

---

- [Nodes and clients]()
- [Javascript APIs]()
- [Backend APIs]()