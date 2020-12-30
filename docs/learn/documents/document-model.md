# Documents

## Data model
The document log is the core underlying data structure for each Ceramic document. It contains a complete history of all updates to the document and its state can be verified by anyone by applying all previously valid transactions in the log.

### Document log
The document log is the core underlying data structure for each Ceramic document. It contains a complete history of all updates to the document and its state can be verified by anyone by applying all previously valid transactions in the log.

### DocID
DocIDs are uniue identifiers for documents on the Ceramic network.

=== "Specification"

    ``` javascript
    lorem ipsum
    ```

=== "Example"

    ``` javascript
    lorem ipsum
    ```

### Records

### VersionID

### Tips
Tips are the current state of a document, represented by its most recent versionID.

## Metadata
Metadata is information that lives in the header of the document.

### Doctypes
Doctypes are drivers for documents on the Ceramic network. They are responsible for specifying core functionality including content requirements and consensus rules. Every document must specify which doctype it is using.

### Controllers
Controllers is a required metadata property for all documents which specifies who is allowed to make updates to the document. If an entity is not included as a controller, its updates to the document will be disregarded by the protocol. The types of controllers allowed is specified by the [doctype](). For example [tile doctypes]() allow [DIDs]() as controllers, while [CAIP-10 links]() allow blockchain addresses.

### Schemas
Schemas are an optional metadata property allowed by certain doctypes, such as the [tile doctype](), which specifies the format and shape of the document's content. All updates to documents that specify a schema must conform to that schema; if not, those updates will be disregarded by the protocol. Schemas themselves are created as documents on Ceramic and the [DocID]() of that schema document should be included in the schema metadata property for the document you are creating.

### Family
Family is an optional metadata property for documets that is used to identify groups of related documents on the network.

### Tags
Tags are optional metadata properties for documets that are used for various classifications. These are flexible and can be used to categorize information in a number of ways.

### Deterministic
Deterministic is an optional metadata boolean that defaults to false. When set to true it omits a random number from being added to the genesis record which allows you to recreate the same genesis record if you create a record with the same metadata and content.

## Content
The content of your document can be anything that is permitted by the doctype specified in the document's metadata. For example the tile doctype allows any JSON content, while the CAIP-10 link doctype is much more strict on allowable content.