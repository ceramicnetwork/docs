# IDX
[IDX](https://developers.idx.xyz) is a JavaScript SDK that provides APIs which make it easy for developers to build applications with user-controlled [streams](../learn/glossary.md#streams) for storing data, and importing user data created on third-party applications. IDX allows users to control their identities and data in a manner independent from any single application, while allowing developers to build data-rich applications without the liability and burden of custodying user data on a centralized server.

> This page mentions that IDX is intended "user" data storage as that is its primary use case. However IDX can be used for other types of subjects represented by DIDs, such as businesses, organizations, applications, assets (NFTs), or devices (IoT).

## **Features**

**DID-compatibility**: IDX does not provide [DIDs](../../learn/glossary.md#dids), but relies on them for decentralized, platform-agnostic identifiers. IDX can work with any DID method that is supported in Ceramic.

**Stream-based storage**: Store data for your users or your application in streams on Ceramic that are controlled by users. Data in streams can be stored in cleartext or encrypted.

**Data hubs**: Whenever a user stores data in a new stream via IDX, the [streamID](../../learn/glossary.md#streamid) of this stream is automatically registered in their index, which is a separate stream that serves as a central catalog of all of their data. The index enables all data associations for the user to exist in one place, which enables any application to discover

**Public, semantic data descriptions**: Application developers can create

**Cross-application data portability**: The combination of DIDs, hubs, streams, and semantic data descriptions allows user data to be stored in an application-agnostic manner and can be used across different applications or interfaces. No application maintains "special permissions", since users are in full control.

**Built on open standards**: IDX builds on open standards for decentralized identity shepherded by the Ceramic community and other related identity communities such as [W3C]() and the [Decentralized Identity Foundation (DIF)]().

**Works with the rest of your Web3 stack**: Since IDX uses DIDs for user identifiers and authentication, IDX can work with whatever wallet your users need to use with the rest of your Web3 application. IDX works with any blockchain Layer 1 or Layer 2 protocol, and users can even link multiple accounts to the same DID to have a unified cross-chain Web3 data identity. Furthermore, since Ceramic streams can be archived on any Web3 persistence platform, you can choose where you users' IDX data is backed up.


## **Protocol design**
Learn about the design and architecture of the IDX protocol, which is implemented by the IDX SDK.

### Index
The index is a stream controlled by the user's DID which stores entries that contain definition to record mappings. Every DID has only one global index and its entries represent the entire catalog of data that belongs to the user. An index is similar to a row in a user table, and enables the decentralized association and discovery of streams that belong to a user.

Example:

```js
{
  "kyz123...456": "ceramic://kyz789...012",
  "kyz345...678": "ceramic://kyz901...234",
  "kyz567...890": "ceramic://kyz123...456",
  "kyz789...012": "ceramic://kyz345...678"
}
```

### Definitions
Definitions are streams created by application developers that store metadata which describes metadata and a schema. Definitions allow records to be discovered and queried using metadata and are similar to a column in a user table. The definitionID is a key in the index.

Example:

```js
{
  name: 'Basic Profile',
  description: 'A simple basic profile.',
  schema: 'ceramic://kyz123...456'
}
```

### Records
Records are streams that store information for a DID. They can directly store content, or they can store foreign key references to external datastores outside of Ceramic. A record is similar to a cell in a user table. The recordURL is a value in the index.

## **How it works**

### Storing data with IDX
1. Application developer creates a TileDocument StreamType that contains a JSON schema, and publishes it to Ceramic.
2. Application developer creates a TileDocument StreamType that contains a definition and includes the [StreamID](../../learn) of the stream containing the schema.
User creates a record that conforms to the definition.
User adds the definitionID and recordID to their index.

### Reading data from IDX

## **Sample Use Cases**

**Authentication secrets**: [3ID Connect]() uses IDX to create a DID-controlled stream which stores encrypted authentication secrets that allows the DID to be authenticated with various blockchain wallets. To achieve this, the 3ID Connect team has created the [3ID Keychain definition]().

**Profile information**: [Self.ID]() uses IDX to create a DID-controlled stream which stores basic profile information for the DID. To achieve this, the Self.ID team had created the [Basic Profile definition]().

## **Implementations**
IDX is available as a JavaScript client library.

## Dependencies

All IDX data is stored in streams controlled by a user's DID, so you will need to separately install a [DID wallet]() or [DID provider]() when using IDX in your application.

## **Usage**

Visit the [**IDX documentation**](https://developers.idx.xyz) to install IDX in your project.

## Identity standards

IDX implements many different decentralized identity standards, and uses others:

- Identity Index (CIP-XX):
- Basic profile (CIP-XX):
- 3ID keychain (CIP-XX):
- Also known as (CIP-XX):
- 

## Learn more

- [Website](https://idx.xyz)
- [Documentation](https://developers.idx.xyz)
- [Discord community](https://chat.idx.xyz)

</br>
</br>
</br>
