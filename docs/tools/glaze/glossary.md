# Glaze Glossary

Common terms used in Glaze packages and tools.

## Schema

A Ceramic TileDocument storing a JSON schema as contents. Schemas are the basis of most Glaze packages and tools functionalities.

## Definition

A TileDocument matching the Definition spec from [CIP-11](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-11/CIP-11.md). Definitions are used in the [Index](#index) of the DID DataStore.

## Tile

In the context of Glaze packages, a Tile refers to a TileDocument with an associated [Schema](#schema).

## Model

A Glaze DataModel, containing a set of [Schemas](#schema) and potentially related [Definitions](#definition) and/or [Tiles](#tile).

## Index

A TileDocument matching the IdentityIndex spec from [CIP-11](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-11/CIP-11.md), used by the DID DataStore to associate [Definition](#definition) IDs to [Record](#record) IDs.

## Record

Data associated to a DID for a given [Definition](#definition) in the DID DataStore. All records are validated using the [Schema](#schemas) declared in the record's [Definition](#definition).
