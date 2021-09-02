# Self.ID

Self.ID is a SDK for building user-centric Web applications using Ceramic and related decentralized technologies, along with a reference application to show and edit profiles.

## **Applications**

- **Self.ID on mainnet**: [https://self.id](https://self.id)
- **Self.ID on Clay Testnet**: [https://clay.self.id](https://clay.self.id)

## **Features**

**Login and connect blockchain wallets**: Self.ID integrates [3ID Connect](../../authentication/3id-did/3id-connect.md) for [authentication](../../build/javascript/authentication.md) to allow users to create and use a [3ID DID](../../authentication/3id-did/method.md) from one or more existing blockchain wallets.

**Basic profiles**: Self.ID allows users to create and/or edit a basic profile for their 3ID DID. To achieve this, Self.ID integrates the [DID DataStore](../glaze/did-datastore.md) to create and query [records](../glaze/overview.md#record) that conform to the [Basic Profile definition and schema (CIP-19)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md).

**Built on open standards**: Self.ID is entirely built on open standards developed by the Ceramic community and the broader decentralized identity ecosystem. All Self.ID functionality can be recreated in any other application, and all data can be queried by any application.

## **Infrastructure**

Self.ID is built using the Ceramic [JS HTTP Client](../../build/javascript/installation.md#js-http-client), [3ID Connect](../../authentication/3id-did/3id-connect.md), [DID DataStore](../glaze/did-datastore.md), and [IdentityLink](../identitylink/overview.md). All Ceramic nodes and IdentityLink services used for Self.ID are hosted by 3Box Labs.

## **Maintainers**

Self.ID is maintained by [3Box Labs](https://3boxlabs.com).

## **SDK**

Many functionalities implemented in the Self.ID application are exposed by the Self.ID SDK, made of the following packages:

### Core

The Core package can be used to read public records in Node and browser environments. You can read more about [configuration](configuration.md) and [read-only interactions](read.md) in the following pages if these docs.

[Core API reference](../../reference/self-id/modules/core.md){: .md-button .md-button }

### Web

The Web package can be used to authenticate and write records in browser environments. You can read more about [authentication and writes](write.md) in the following pages if these docs.

[Web API reference](../../reference/self-id/modules/web.md){: .md-button .md-button }

### Utility packages

Additional [utility packages](utilities.md) are provided by the SDK to help support more specific use-cases.
