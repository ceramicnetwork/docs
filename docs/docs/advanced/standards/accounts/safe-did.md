# **Gnosis Safe DID Accounts**

---

> This page is a stub. We're working on adding more content to help you build with Safe DIDs.

!!! warning ""

    **⚠️ Safe DID is experimental.** Please reach out in [Discord](https://chat.ceramic.network) to provide feedback or get help.

The Gnosis Safe DID Method (CIP-101) is an account that can perform transactions on streams. Safe DID accounts are controlled by the current members of a Gnosis Safe smart contract. Safe DID is on the W3C's official DID method registry and is fully compliant with decentralized identity standards, however they are still highly experimental, so use at your own risk.

## **What is a Safe account?**

---

The Safe DID Method turns every Gnosis Safe contract into a Ceramic account capable of sending transactions to streams on Ceramic. It is similar in design and usage to the NFT DID account method. 

Write permissions for streams whose [controller](../../../../learn/glossary.md#controllers) is set to a Safe DID are restricted to the DIDs of the blockchain accounts that control the Gnosis Safe contract. When Gnosis Safe controller permissions change on-chain, so do the write permissions to streams owned by the Safe. 

## **Specification**

---

Read the [Safe DID Method (CIP-101) Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-101/CIP-101.md) for the full specification.
