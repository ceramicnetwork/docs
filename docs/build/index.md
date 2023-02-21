# **Build Applications**

---

To build applications on Ceramic, use a Ceramic-powered database.

## **Featured Databases**

---

### [**ComposeDB: A Graph DB for Web3 Apps →**](https://composedb.js.org/)

ComposeDB is the latest and greatest way to build apps with composable data on Ceramic's data layer. It is a decentralized, verifiable graph database that supports both Javascript and GraphQL.

## **Legacy Databases**

---

### [**IDX & Self.ID →**](../reference/self-id/index.md)

IDX, a key-value store DB, is a legacy alternative to ComposeDB. Self.ID is a framework that made it easier to use IDX. These used to be recommended databases for building apps on Ceramic, but they have since been replaced by ComposeDB.

## **Build a new Database**

---

If the database solutions above don't suit your needs, you can always build a new database powered by Ceramic by using the [**Ceramic client**](../reference/core-clients/ceramic-http.md). Clients are a lower-level way to connect to the Ceramic network; they come without much pre-configuration, so you'll need to put at least a few different pieces together yourself.

## **Additional Notes**

---

### System Requirements

- Ceramic clients and tools using them are designed for environments supporting [ECMAScript 11th edition](https://262.ecma-international.org/11.0/), commonly referred to as ES2020.
- Most packages are exposed as ECMAScript Modules (ESM), which are supported by most modern browsers and recent versions of Node.
- We recommend Node v16 (LTS), as it is the version used for tests in most Ceramic packages.
