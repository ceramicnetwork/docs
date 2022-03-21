# **Clients**

---

## **Types of clients**

---

### **Ceramic clients**

Ceramic clients are libraries that allow your application to communicate with a Ceramic node. Different clients may choose to implement different high-level, language-specific developer APIs. Before submitting requests to a Ceramic node, clients translate those API calls into the standard [Ceramic HTTP API](../http/api.md), which it uses to actually communicate with a Ceramic node.

### **Account clients**

Account clients are libraries that allow your application to recognize users, authenticate, and perform other account-related functionality such as signing transactions and encrypting data.

## **Available clients**

---

When building with Ceramic clients instead of using a [framework](../frameworks/index.md), be sure to install both a Ceramic client and an account client.

### [**JS Ceramic HTTP Client →**](../../reference/core-clients/ceramic-http.md)

The Ceramic JS HTTP client is a Ceramic client that can be used in browsers and Node.js environments to connect your application to a Ceramic node. It is actively maintained by 3Box Labs and supports the latest Ceramic features. This is the recommended Ceramic client to build with for most applications.

<!-- ### [**JS Ceramic Core Client →**]()

The Ceramic JS Core client is a Ceramic client that can be used in Node.js environments and includes a local Ceramic node. It is actively maintained by 3Box Labs and supports the latest Ceramic features. This is the recommended Ceramic client to use in test environments and other places where you can directly run a Ceramic node, but is not recommended for web applications. -->

### [**DID JSON-RPC Client →**](../../reference/core-clients/did-jsonrpc.md)

The DID JSON-RPC Client is an account client that provides a simple JS API for interacting with Ceramic accounts. It is actively maintained by 3Box Labs and supports all account types.
