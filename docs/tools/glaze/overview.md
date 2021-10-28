# Glaze

Glaze is a set of libraries and tools built on top of Ceramic to help support some common use-cases.

!!! warning ""

    :octicons-stop-16: Before reading further and getting started using Glaze packages, you should be familiar with other Ceramic concepts previously presented, such as [DIDs](../../../learn/glossary/#dids), [authentication](../../../build/javascript/authentication/), [schemas](../../../build/javascript/quick-start/#5-create-a-schema), [streams](../../../learn/glossary/#streams) and [stream types](../../../learn/glossary/#streamtypes).

## Tile Loader

The Tile Loader library provides a thin abstraction over the [`TileDocument`](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_stream_tile.tiledocument-1.html) stream client to add the following performance improvements:

- Batching: grouping concurrent loading of multiple streams in a single call.
- Caching: storing already loaded streams client-side to avoid making repeated requests to the Ceramic node.

[Tile Loader documentation](tile-loader.md){: .md-button }

## DataModel

A DataModel is a concept used in various Glaze libraries and tools to represent a set of Ceramic streams that an application interacts with, mainly in the following ways:

1. Creation and edition: during development, creating or importing existing streams that will be used by the app at runtime.
2. Publication: the process of deploying the necessary streams used by an app to a given Ceramic node.
3. Interaction: at runtime, referencing streams by human-friendly aliases rather than stream IDs.

[DataModel documentation](datamodel.md){: .md-button }

## DID DataStore

A DID DataStore is a runtime library leveraging a DataModel to associate known entries to a given DID, enabling DID-based storage and discovery across any application.

[DID DataStore documentation](did-datastore.md){: .md-button }

## Development tools

### DevTools library

The DevTools library provides APIs to help with the management of DataModels, notably for creation, edition and publication of models.

[DevTools documentation](development.md#devtools-library){: .md-button }

### CLI

The Glaze CLI provide commands for common interactions with DataModels and DID DataStores to help support development flows.

[CLI documentation](development.md#cli){: .md-button }
