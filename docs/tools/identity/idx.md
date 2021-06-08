# IDX
IDX is a JavaScript SDK that provides APIs which make it easy for developers to build applications with user-controlled [streams](../learn/glossary.md#streams) for storing data, and importing user data created on other applications. 

## **Built on open standards**

IDX builds on open standards for decentralized identity shepherded by the Ceramic community and other related identity communities such as W3C and the [Decentralized Identity Foundation (DIF)].


## Dependencies

All IDX data is stored in streams controlled by a user's DID, so you will need to separately install a [DID wallet]() or [DID provider]() when using IDX in your application.

## **Architecture**
Learn about the architecture of IDX.

### Index
The index is a stream controlled by the user's DID which stores entries that contain definition to record mappings. Every DID has only one global index and its entries represent the entire catalog of data that belongs to the user. An index is similar to a row in a user table, and enables the decentralized association and discovery of streams that belong to a user.

### Definitions
Definitions are streams created by application developers that store metadata which describes metadata and a schema. Definitions allow records to be discovered and queried using metadata and are similar to a column in a user table. The definitionID is a key in the index.

### Records
Records are streams that store information for a DID. They can directly store content, or they can store foreign key references to external datastores outside of Ceramic. A record is similar to a cell in a user table. The recordURL is a value in the index.

## **Sample Use Cases**

**Authentication secrets**: [3ID Connect]() uses IDX to create a DID-controlled stream which stores encrypted authentication secrets that allows the DID to be authenticated with various blockchain wallets. To achieve this, the 3ID Connect team has created the [3ID Keychain definition]().

**Profile information**: [Self.ID]() uses IDX to create a DID-controlled stream which stores basic profile information for the DID. To achieve this, the Self.ID team had created the [Basic Profile definition]().


## **Implementations**
IDX is available as a JavaScript client library.


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
