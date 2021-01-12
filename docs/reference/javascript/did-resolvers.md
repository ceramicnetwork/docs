# DID Resolvers
A DID resolver is a small library that takes a DID string and returns a DID Document. Below you can find references for the main *did-resolver* package as well as plugins for specific DID Methods.

## DID Resolver
The main library needed to start resolving DIDs is the `did-resolver` package. It is maintained by the Decentralized Identity Foundation (DIF) and can be found at their github:

[:octicons-file-code-16: decentralized-identity/did-resolver](https://github.com/decentralized-identity/did-resolver){:target="_blank"}

## 3ID DID Resolver
The 3ID DID Method is a DID that is implemented on top of Ceramic using the *tile* doctype. The 3ID DID resolver uses Ceramic to resolve DIDs.

[:octicons-file-code-16: ceramicnetwork/3id-did-resolver](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/3id-did-resolver){:target="_blank"}

## Key DID Resolver
The Key DID Method is the most simple DID method. It simply encodes a public key in the DID string, and when resolved converts this public key into
a DID Document.

[:octicons-file-code-16: ceramicnetwork/key-did-resolver](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/key-did-resolver){:target="_blank"}

[:octicons-file-code-16: did:key specification](https://w3c-ccg.github.io/did-method-key/){:target="_blank"}

<br />
<br />
<br />
