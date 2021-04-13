# Key DID

Key DID is one of a few DID methods supported by Ceramic that can be used to perform authenticated writes to streams that require DIDs. This includes the two default StreamTypes: Tile Documents and CAIP-10 Links.

The DID document for a Key DID is statically generated from any Ed25519 cryptographic key pair.

stored natively on Ceramic as a Tile Document StreamType, allowing for mutability and enabling it to securely handle key rotations. This is a desireable property particularly for end user DIDs. Additionally since 3ID is native to Ceramic, developers do not need to operate additional infrastructure or rely on other external technologies.

Usage
See the Authentication page to learn how to use the 3ID DID method in your Ceramic project.

Other DID methods
Ceramic supports a few different DID methods for authentication. Below find links to the others:

Key DID
NFT DID (coming soon)
Safe DID (coming soon)
Technical specifications
See CIP-79 for the full technical specification of the 3ID DID method
See the W3C standard for DIDs
