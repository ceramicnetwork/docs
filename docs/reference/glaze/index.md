# **Glaze Suite**

---

Glaze suite includes a collection of tools for building applications on Ceramic, including data model management tools, runtime libraries for user-centric data storage and retrieval, and client-side tools for caching data from the network. Glaze modules can be used separately, but are best when used together.

![](../../images/glaze.png)

!!! warning ""

    :octicons-stop-16: Before reading further and getting started using Glaze packages, you should be familiar with other Ceramic concepts previously presented, such as [DIDs](../../learn/glossary.md#dids), [authentication](../../build/javascript/authentication.md), [schemas](../../build/javascript/quick-start.md#5-create-a-schema), [streams](../../learn/glossary.md#streams) and [stream types](../../learn/glossary.md#streamtypes).

## **Data model management**

---

### [**Deploy from JavaScript →**](../../tools/glaze/development.md)

The devtools module provides JavaScript APIs for managing and deploying the data models used by your Ceramic application.

### [**Deploy from the CLI →**](../../tools/glaze/deploy-from-cli.md)

The Glaze CLI provides a set of command-line tools for managing and deploying the data models used by your Ceramic application.

## **Build with data models**

---

### [**User-centric storage →**](../../tools/glaze/did-datastore.md)

The DID DataStore module provides read and write APIs allowing applications to interact with user data based on data model. DID DataStore also makes it possible for applications to discover all information about a user in one place, forming the basis for user-centric data composability on Ceramic.

### [**Data model aliasing →**](../../tools/glaze/datamodel.md)

The DataModel module provides human-readable aliasing for data models at runtime, making it easier to use the DID DataStore API.

## **Client-side caching**

---

### [**Tile loader →**](../../tools/glaze/tile-loader.md)

The tile loader module provides client-side batching and caching for Ceramic data, improving the performance of retrieving data from the network in order to populate your application.
