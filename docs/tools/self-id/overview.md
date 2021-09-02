# Self.ID

[Self.ID](https://self.id) is a web application that provides a UI for basic identity functionality for [3ID DIDs](../../authentication/3id-did/method.md). Any project that wants their users to have basic identity functionality without wanting to integrate editing features directly into their UI can instead direct users to Self.ID. With Self.ID, users havs a simple interface for managing their core identity information, while third-party applications can query this data using the [IDX SDK](../idx/overview.md).

## **Applications**

- **Self.ID on mainnet**: https://self.id
- **Self.ID on Clay Testnet**: https://clay.self.id

!!! warning ""

    Self.ID is under active development. Please report issues in the #self-id channel in the [Ceramic Discord](https://chat.ceramic.network) server.

## **Features**

**Login and connect blockchain wallets**: Self.ID integrates with the [3ID Connect SDK](../../authentication/3id-did/3id-connect.md) for [authentication](../../build/javascript/authentication.md) to allow users to create and use a [3ID DID](../../authentication/3id-did/method.md) from one or more existing blockchain wallets.

**Basic profiles**: Self.ID allows users to create and/or edit a basic profile for their 3ID DID. To achieve this, Self.ID integrates with the [IDX SDK](../idx/overview.md) to create and query [IDX records](../idx/overview.md#records) that conform to the [Basic Profile definition and schema (CIP-19)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md).

**Link Web2 accounts** (coming soon): Self.ID allows users to link many different Web2 accounts to their 3ID. To achieve this, Self.ID integrates with the [IDX SDK](../idx/overview.md) to create and query [IDX records](../idx/overview.md#records) that conform to the [Also Known As (AKA) definition and schema (CIP-23)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md). Each user's AKA record contains a list of Web2 accounts and their corresponding proofs, which are verifiable claims issued by an [IdentityLink](../identitylink/overview.md) service.

**Built on open standards**: Self.ID is entirely built on open standards developed by the Ceramic community and the broader decentralized identity ecosystem. All Self.ID functionality can be recreated in any other application, and all data can be queried by any application.

## **Infrastructure**

Self.ID is built using the Ceramic [JS HTTP Client](../../build/javascript/installation.md#js-http-client), [3ID Connect](../../authentication/3id-did/3id-connect.md), [IDX SDK](../idx/overview.md), and [IdentityLink](../identitylink/overview.md). All Ceramic nodes and IdentityLink services used for Self.ID are hosted by 3Box Labs.

## **Maintainers**

Self.ID is maintained by [3Box Labs](https://3boxlabs.com).
