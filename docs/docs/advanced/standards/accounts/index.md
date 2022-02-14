# **Ceramic Accounts**

---

Ceramic uses the [Decentralized Identifier (DID) standard]() for user accounts. Decentralized identifiers provide an abstraction from individual cryptographic accounts, such as blockchain accounts, enabling cross-chain interoperability and multi-account to identity support.

## **Supported account types**

---

Below find the decentralized identifier (DID) standards currently supported as user accounts on Ceramic:

| NAME | DESCRIPTION | Status | 
| ----- | ----------- | --- | 
| [3ID DID (CIP-79)]() | A Ceramic-native account that supports mutations in the DID Document, enabling the association of multiple wallets to the account | ✅ Production |
| [Key DID]() | A Ceramic account that can only be associated with one wallet, which can never be changed | ✅ Production |
| [NFT DID (CIP-94)]() | A Ceramic account that can be controlled by the current owner of a given NFT (non-fungible token) | ⚠️ *Experimental* |
| [Safe DID (CIP-101)]() | A Ceramic account that can be controlled by the current members of a Gnosis Safe smart contract | ⚠️ *Experimental*

## **Building with accounts**

---

In order for users to perform transactions on Ceramic they need an account. Your application can control which accounts it supports. 

- When building with a Ceramic client, you should install and configure the [DID JSON-RPC client](), which will instruct you how to add support for various account types.
- When building with a framework such as the [Self.ID SDK](), you don't need to worry about accounts, it will use 3ID DID.

