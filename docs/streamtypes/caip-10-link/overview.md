# CAIP-10 Link

CAIP-10 Link is a StreamType that stores a cryptographically verifiable proof that links a blockchain address to a DID.

## Use cases

A DID can have an unlimited number of CAIP-10 Links that publicly bind it to many different addresses on many different L1 and L2 blockchain networks.

## Design

- Authentication: DIDs
- Ordering: Anchor records checkpointed into a blockchain (via blockheight)
- Conflict resolution: Earliest anchor wins (EAW)

</br>
</br>
</br>
