# Self.ID

Self.ID is a web application that provides users with basic identity features for their [3ID DID](). Self.ID can be used by any application that wants their users to have basic identity functionality without needing to integrate write functionality directly into their application UI. With Self.ID, users havs a simple interface for managing their core identity information, while third-party applications can query this data using the API provided by the [IDX SDK](). Self.ID is based on open standards developed by the Ceramic community and the broader decentralized identity ecosystem.

## **Usage**

- **Mainnet**: https://self.id
- **Clay Testnet**: https://clay.self.id

!!! warning ""

    Self.ID works, but is under active development. Please report issues in the #self-id channel in the [Ceramic Discord](https://chat.ceramic.network) server.

## **Features**

**Login with blockchain wallets**: Self.ID integrates with the [3ID Connect]() SDK to allow users to create and/or authenticate a [3ID DID]() using one or more existing blockchain wallets.

**Basic profiles**: Self.ID allows users to create and/or edit a basic profile for their 3ID DID. To achieve this, Self.ID integrates with the [IDX SDK]() to create and query [IDX records]() that conform to the [Basic Profile definition and schema (CIP-XX)]().

**Link Web3 accounts**: Self.ID allows users to link many different blockchain accounts to their 3ID. To achieve this, Self.ID integrates with the [IDX SDK]() to create and query [IDX records]() that conform to the [Crypto Accounts definition and schema (CIP-XX)](). Each user's Crypto Accounts record in IDX contains a list of [streamIDs]() for the [Caip10Links]() for their 3ID.

**Link Web2 accounts**: Self.ID allows users to link many different Web2 accounts to their 3ID. To achieve this, Self.ID integrates with the [IDX SDK]() to create and query [IDX records]() that conform to the [Also Known As (AKA) definition and schema (CIP-XX)](). Each user's AKA record in IDX contains a list of Web2 accounts and their corresponding proofs, which are verifiable claims issued by an [IdentityLink]() service.

## **Infrastructure**

Self.ID is built using the Ceramic [JS HTTP Client](), [3ID Connect](), [IDX SDK](), and [IdentityLink](). All Ceramic nodes and IdentityLink services used for this application are hosted by 3Box Labs.

## **Maintainers**

Self.ID is maintained by [3Box Labs](https://3boxlabs.com).

</br>
</br>
</br>
