# NFT DID Method

!!! warning ""
    
    **NFT DID is alpha.** Please reach out in [Discord](https://chat.ceramic.network) to express a feedback.

The [NFT DID Method (CIP-94)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-94/CIP-94.md) is a [DID method](../../learn/glossary.md#did-methods)
that can be used to [authenticate](../../build/javascript/authentication.md) to Ceramic to perform [writes](../../build/javascriptwrites.md)
to [streams](../../learn/glossary.md#streams) that rely on DIDs for authentication.
NFT DID is a lightweight DID method with permissions that change based on on-chain NFT asset ownership.

The NFT DID Method turns every NFT into a DID capable of controlling streams on Ceramic.
Write permissions for streams whose [controller](../../learn/glossary.md#controllers) is set to an NFT DID
are restricted to the DID of the blockchain account that currently owns the NFT.
When the NFT changes ownership on-chain, so do the write permissions.
NFT DID is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

## **Rationale**

One can find NFT tokens used in the wild for providing access control to articles, communities,
and other form of gated contend. An NFT token can be used to control content of Ceramic stream as well.
With implementation of [NFT DID Method (CIP-94)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-94/CIP-94.md)
we can translate an NFT into DID, which can actually control a Ceramic stream.

In general, an NFT owner becomes a stream controller and have full read-write access to it.
If the NFT gets transferred, the rights move to a new owner. We heard of the following scenarios for this:

1. External NFT meta-data, like annotation. An NFT owner can annotate an NFT with a story behind an artefact, or, for example, carbon offsetting certificate.
2. NFT as smart-contract enabled access control. An NFT owner creates a Ceramic stream, then some smart-contract logic is used to transfer NFT to a different person, and she now can continue the work on the stream.

## **How to use**

To use an NFT as a Ceramic stream controller, one has to include [nft-did-resolver](https://www.npmjs.com/package/nft-did-resolver) in Ceramic node.
Ceramic upstream includes it by default. Until a new version is released, one has to build your own node from our GitHub repository though.

One creates an NFT-controlled stream by setting a stream controller to `did:nft` DID URL. For example,

```
const didNFT =
    "did:nft:eip155:4_erc721:0xe2a6a2da2408e1c944c045162852ef2056e235ab_1";
const tile = await TileDocument.create(ceramic, {foo: "blah"}, {controllers: [didNFT]})
```

Now that tile we just created can only be controlled by an NFT owner.

Here `didNFT` string is a DID URL that references ERC721 token `1` on contract `0xe2a6a2da2408e1c944c045162852ef2056e235ab` deployed to Rinkeby (`eip155:4`). This should reference your NFT.
We provide a helper function `createNftDidUrl` to create such a string.

```
import { createNftDidUrl } from 'nft-did-resolver'
const didNFT = createNftDidUrl({
  chainId: 'eip155:1',
  namespace: 'erc721:0x1234567891234567891234567891234596351156',
  tokenId: '1',
}) // "did:nft:eip155:4_erc721:0xe2a6a2da2408e1c944c045162852ef2056e235ab_1"
```

## **Under the hood**

When resolving did-nft, Ceramic and did-nft-resolver do the following.
We query a blockchain (via subgraph) for the NFT owners. Then for each NFT owner we find a corresponding [CAIP10Link (CIP-7)](../../streamtypes/caip-10-link/overview.md) stream.
It provides a link from blockchain account to Ceramic 3id DID.

![NFT-DID Relationship](../../images/nft-did-link.png)

When a signature is made by usual 3ID, we verify if there is indeed such a connection from NFT to 3id.

If you are a dApp developer who wishes to use NFT-DID, you have to make sure that your users
have that connection via [CAIP10Link (CIP-7)](../../streamtypes/caip-10-link/overview.md).
If a user authenticates via 3id-connect, as usually is the case, such a connection is created automatically.

## **Constraints**

We support only [ERC721](https://eips.ethereum.org/EIPS/eip-721) and [ERC1155](https://eips.ethereum.org/EIPS/eip-1155) tokens for now.
We support Rinkeby, Etherem mainnet and Polygon networks by default. If you need other networks, please, consult with [nft-did-resolver](https://github.com/ceramicnetwork/nft-did-resolver) README,
and update configuration of your Ceramic node. There you can find parameters required for a new network.
Namely, it needs three [subgraphs](https://thegraph.com): for blocks, for erc721 and erc1155 tokens, and a "skew", which is a typical block time.


## **Specification**
Read the [NFT DID Method (CIP-94) Specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-94/CIP-94.md).

NFT DID is on the W3C's official DID method registry and is fully compliant with decentralized identity standards.

</br></br></br>
