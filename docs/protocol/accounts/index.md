# Accounts

User owned Ceramic accounts

## Overview

---

User owned data requires an account model that is both core to the protocol and general enough to support the wide diversity of possible account models and real world scenarios. Accounts are identified by Decentralized Identifiers, a general and extensible method to represent unique account strings, resolve public keys, and other account info or key material. Object-Capabilities are used to permission and authorize stream writes from one account to another, this may include session keys, applications and managing organization access. 

## Decentralized Identifiers

---

Decentralized Identifiers (DIDs) are used to represent accounts. DIDs are identifiers that enable verifiable, decentralized digital identities. They require no centralized party or registry and are extremely extensible, allowing a variety of implementations and account models to exist. 

[Identifiers](decentralized-identifiers.md)

## Permissions

---

Permissions allow one account to delegate stream access to another account. While the current model is simple and minimal, it is descriptive enough to follow the rule of least privilege and limit the access that is delegated to another account. 

[Permissions](permissions.md)

## Object-capabilities

---

Object-Capabilities or CACAO are the technical feature and implementation that enables support for permissions and a general and powerful capability-based authorization system over streams.

[Object Capabilities](object-capabilities.md)