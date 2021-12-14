# Safe DID Method

!!! warning ""

    **Safe DID is under active development and not yet supported in Ceramic.** Please reach out in [Discord](https://chat.ceramic.network) to express a desire to use it in your project or to contribute.

The [Safe DID Method (CIP-101)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-101/CIP-101.md) is a [DID method](../../learn/glossary.md#did-methods) that can be used to [authenticate](../../build/javascript/authentication.md) to Ceramic to perform [writes](../../build/javascript/writes.md) to [streams](../../learn/glossary.md#streams) that rely on DIDs for authentication. Safe DID is a lightweight DID method with permissions that change based on on-chain [Gnosis Safe](https://gnosis-safe.io/) contract permissions.

## **How it works**

The Safe DID Method turns every Gnosis Safe contract into a DID capable of controlling streams on Ceramic. Write permissions for streams whose [controller](../../learn/glossary.md#controllers) is set to a Safe DID are restricted to the DIDs of the blockchain accounts that control the Gnosis Safe contract. When Gnosis Safe controller permissions change on-chain, so do the write permissions to streams owned by the Safe. Safe DID is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

## **Specification**

Read the [Safe DID Method (CIP-101) Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-101/CIP-101.md).
