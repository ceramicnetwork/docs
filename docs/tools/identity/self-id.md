# Self.ID

[Self.ID](https://self.id) is a web application that provides a UI for basic identity functionality for [3ID DIDs](../../authentication/3id-did/method.md). Any project that wants their users to have basic identity functionality without wanting to integrate editing features directly into their UI can instead direct users to Self.ID. With Self.ID, users havs a simple interface for managing their core identity information, while third-party applications can query this data using the [IDX SDK](./idx.md).

## **Applications**

- **Self.ID on mainnet**: https://self.id
- **Self.ID on Clay Testnet**: https://clay.self.id

!!! warning ""

    Self.ID works, but is under active development. Please report issues in the #self-id channel in the [Ceramic Discord](https://chat.ceramic.network) server.

## **Features**

**Login with blockchain wallets**: Self.ID integrates with the [3ID Connect SDK](../../authentication/3id-did/3id-connect.md) for [authentication](../../learn/build/authentication.md) to allow users to create and use a [3ID DID](../../authentication/3id-did/method.md) from one or more existing blockchain wallets.

**Basic profiles**: Self.ID allows users to create and/or edit a basic profile for their 3ID DID. To achieve this, Self.ID integrates with the [IDX SDK](./idx.md) to create and query [IDX records](./idx.md#records) that conform to the [Basic Profile definition and schema (CIP-19)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md).

**Link Web3 accounts**: Self.ID allows users to link many different blockchain accounts to their 3ID. To achieve this, Self.ID integrates with the [IDX SDK](./idx.md) to create and query [IDX records](./idx.md#records) that conform to the [Crypto Accounts definition and schema (CIP-21)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md). Each user's Crypto Accounts record in IDX contains a list of [streamIDs](../../learn/glossary.md#streamid) for the [Caip10Links](../../streamtypes/caip-10-link/overview.md) of their 3ID.

**Link Web2 accounts**: Self.ID allows users to link many different Web2 accounts to their 3ID. To achieve this, Self.ID integrates with the [IDX SDK](./idx.md) to create and query [IDX records](./idx.md#records) that conform to the [Also Known As (AKA) definition and schema (CIP-23)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md). Each user's AKA record contains a list of Web2 accounts and their corresponding proofs, which are verifiable claims issued by an [IdentityLink](./identitylink.md) service.

**Built on open standards**: Self.ID is entirely built on open standards developed by the Ceramic community and the broader decentralized identity ecosystem. All Self.ID functionality can be recreated in any other application, and all data can be queried by any application.

## **Infrastructure**

Self.ID is built using the Ceramic [JS HTTP Client](../../clients/javascript/http.md), [3ID Connect](../../authentication/3id-did/3id-connect.md), [IDX SDK](./idx.md), and [IdentityLink](./identitylink.md). All Ceramic nodes and IdentityLink services used for Self.ID are hosted by 3Box Labs.

## **Maintainers**

Self.ID is maintained by [3Box Labs](https://3boxlabs.com).

</br>
</br>
</br>
