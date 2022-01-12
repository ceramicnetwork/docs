# CIP-7: CAIP-10 Link

CAIP-10 Link is a StreamType that stores a cryptographically verifiable proof that links a blockchain address to a DID.

## Overview

- The CAIP-10 Link StreamType leverages the [Chain Agnostic Improvement Proposal (CAIP) 10 standard](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) to represent blockchain accounts and cryptographic signatures to associate these accounts to a DID.
- Links are one way only: from a blockchain account to a DID. It is not possible to know what blockchain accounts are associated to a DID using a CAIP-10 Link. [TODO: link to crypto accounts definition]. It also does not mean a given blockchain account owns the linked DID, only that the owner of the blockchain account created this link.
- A DID can have an unlimited number of CAIP-10 Links that publicly bind it to many different addresses on many different L1 and L2 blockchain networks.

## Use-cases

The primary use-case of a CAIP-10 Link is to create a pointer from a CAIP-10 account to a DID. This is notably useful to use blockchain accounts as entry-points for applications using Ceramic, in order to

## Structure

### Content

The content of a CAIP-10 Link available to consumers is the `string` value of a DID or `null` if no DID is linked.

Internally, the stream commits contain a signed link proof. [TODO: point to blockchain utils linking? any dedicated page?].

### Metadata

Metadata for CAIP-10 Links is defined by the protocol and not editable by consumers. It contains the blockchain account (CAIP-10 string) as only entry in `controllers` and a `family` using the chain ID.

## Reference

- [CIP-7 standard](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-7/CIP-7.md)
- [JavaScript client implementation](../../../../reference/stream-programs/caip10-link.md)
