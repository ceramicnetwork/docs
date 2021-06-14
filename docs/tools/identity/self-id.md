# Self.ID

Self.ID is a web application that provides users with basic identity features for their [3ID DID](). Self.ID can be used by any application that wants their users to have basic identity functionality without needing to integrate write functionality directly into their application UI. With Self.ID, users havs a simple interface for managing their core identity information, while third-party applications can query this data using the API provided by the [IDX SDK](). Self.ID is based on open standards developed by the Ceramic community and the broader decentralized identity ecosystem.

## **Usage**

- **Mainnet**: https://self.id
- **Clay Testnet**: https://clay.self.id

!!! warning ""

    Self.ID works, but is under active development. Please report issues in the #self-id channel in the [Ceramic Discord](https://chat.ceramic.network) server.

## **Features**

**Login with blockchain wallets**: Self.ID integrates with the [3ID Connect]() SDK to allow users to create and/or authenticate a [3ID DID]() using one or more existing blockchain wallets.

**Create a basic profile for a DID**: Self.ID allows users to create and/or edit a basic profile for their 3ID DID. This is stored in an [IDX record]() that conforms to the [Basic Profile definition and schema (CIP-XX)]().

**Link Web3 accounts to a DID**: Self.ID allows users to link many different blockchain accounts to their 3ID. Each blockchain account link is a Caip10Link stream, and a list of streamIDs for the user's Caip10Links is stored in a stream that conforms to the [Crypto Accounts definition and schema (CIP-XX)](). Applications can use this data to query which blockchain accounts belong to a DID (`DID -> blockchain account(s)`), or alternatively to query a DID (and by extension all its other data) using any of these blockchain accounts

**Link Web2 accounts to a DID**: Self.ID integrates with [IdentityLink]() to issue verifiable claims that prove the 3ID owns various Web2 accounts. These proofs are publicly stored in the 3ID's 

**Edit or query from any application**: Self.ID is not a walled-garden for identity information. All data created on Self.ID can be openly queried and/or edited from within any application that integrates the IDX SDK. Based on IDX SDK.


## **Implementation**

### Authentication

- [**3ID DID Method (CIP-XX)**]()

- [**3ID Connect**]():

### Identity management

- [**IDX (CIP-11) SDK**]():

### Profiles

- [**Basic Profile definition and schema (CIP-XX)**]()

### Social accounts

- [**IdentityLink**]():
- [**Also Known As (AKA) definition and schema (CIP-XX)**]():

### Crypto acounts

- [**Caip10Link StreamType**]():
- [**Crypto Accounts definition and schema (CIP-XX)**]():
- [**3ID Keychain definition and schema (CIP-XX)**]():

## Maintainers

Self.ID is maintained by [3Box Labs](https://3boxlabs.com).

</br>
</br>
</br>
