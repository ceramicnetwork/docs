# StreamTypes
Each [stream](../learn/glossary.md#streams) on Ceramic must be created using a specific [StreamType](../learn/glossary.md#streamtypes). Different StreamTypes have different APIs for performing [writes](../build/writes.md). To create and/or update streams, see the API page for the respective StreamTypes below.

## **Responsibilities**
StreamTypes are programmable functions responsible for processing all updates to the stream and enforcing logic such as data structure, requirements for [authentication](../learn/glossary.md#authentication), the content of the stream's [commits](../learn/glossary.md#commits), and its [conflict resolution strategy](../learn/glossary.md#conflict-resolution-strategy). 

## **Default StreamTypes**
These StreamTypes are included in every Ceramic client and node implementation by default. Applications can make use of them without any additional work by simply using their APIs.

### TileDocument
TileDocuments are streams that store JSON content and support JSON schema validation. They can serve as JSON document stores for arbitrary application data.

[**Overview**](./tile-document/overview.md) | [**API**](./tile-document/api.md)

### Caip10Link
Caip10Links are streams that store proofs which publicly link a blockchain account to a DID. They serve to link one or more Web3 accounts to a DID.

[**Overview**](./caip-10-link/overview.md) | [**API**](./caip-10-link/api.md)

## **Custom StreamTypes**
It is possible to develop custom StreamTypes for use within your application. We will soon add a guide describing how this works.


</br>
</br>
</br>
