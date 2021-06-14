# NFT DID Method
NFT DID is a [DID method](../../learn/glossary.md#did-methods) that is under development, but not yet supported in Ceramic. It can be used to [authenticate](../../build/authentication.md) to a Ceramic client to perform [writes](../../build/writes.md) to [streams](../../learn/glossary.md#streams) that rely on DIDs for authentication. The NFT DID Method turns every NFT into a DID capable of controlling streams on Ceramic. Write permissions for streams whose [controller](../../learn/glossary.md#controllers) is set to an NFT DID are restricted to the DID of the blockchain account that currently owns the NFT. When the NFT changes ownership on-chain, so do the write permissions. NFT DID is on the W3C's [official DID method registry]() and is fully compliant with decentralized identity standards.

## **Specification**
Read the [NFT DID Method (CIP-94) Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-94/CIP-94.md).

</br></br></br>
