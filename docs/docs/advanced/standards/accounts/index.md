# **Ceramic Accounts**

---

Ceramic uses the [Decentralized Identifier (DID) standard](../../../../learn/glossary.md#dids) for user accounts. Decentralized identifiers provide an abstraction from individual cryptographic accounts, such as blockchain accounts, enabling cross-chain interoperability and multi-account to identity support.

## **Supported account types**

---

Below find the decentralized identifier (DID) standards currently supported as user accounts on Ceramic:

| NAME                                | DESCRIPTION                                                                                                                       | Status            |
| ----------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- | ----------------- |
| [DID PKH (CIP-79)](https://did.js.org)    | A Ceramic account that natively supports blockchain accounts, default usage with did-session allows capabality and session usage  | ✅ Production     |
| [3ID DID (CIP-79)](./3id-did.md)    | A Ceramic-native account that supports mutations in the DID Document, enabling the association of multiple wallets to the account | ✅ Production     |
| [Key DID](./key-did.md)             | A Ceramic account that can only be associated with one wallet, which can never be changed                                         | ✅ Production     |
| [NFT DID (CIP-94)](./nft-did.md)    | A Ceramic account that can be controlled by the current owner of a given NFT (non-fungible token)                                 | ⚠️ _Experimental_ |
| [Safe DID (CIP-101)](./safe-did.md) | A Ceramic account that can be controlled by the current members of a Gnosis Safe smart contract                                   | ⚠️ _Experimental_ |

## **Building with accounts**

---

In order for users to perform transactions on Ceramic they need an account. Your application can control which accounts it supports.

- When building with a Ceramic client, you should install and configure the [DID JSON-RPC client](../../../../reference/core-clients/did-jsonrpc.md), which will instruct you how to add support for various account types.
- When building with a framework such as the [Self.ID SDK](../../../../reference/self-id/index.md), you don't need to worry about accounts, it will use DID PKH with DID Session.
