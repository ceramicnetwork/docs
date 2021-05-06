# 3ID DID

3ID is one of a few DID methods supported by Ceramic that can be used to perform authenticated writes to streams that require DIDs. This includes the two default StreamTypes: Tile Documents and CAIP-10 Links.

When 3IDs are used in conjunction with IDX and the 3ID Keychain (as is implemented in 3ID Connect), a 3ID can easily be controlled with any number of blockchain accounts from any L1 or L2 network. This provides a way to unify a user's identity across all other platforms.

## Usage

See the [Authentication](https://developers.ceramic.network/build/authentication/) page to learn how to use the 3ID DID method in your Ceramic project.

## Design

The DID document for a 3ID is stored natively on Ceramic as a Tile Document StreamType, allowing for mutability and enabling it to securely handle key rotations. This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies.

See [CIP-79](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-79/CIP-79.md) for the full technical specification of the 3ID DID method.


## Other DID methods
Ceramic supports a few different DID methods for authentication. Below find links to the others:

- [Key DID](./key.md)
- NFT DID (coming soon)
- Safe DID (coming soon)
