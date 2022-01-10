# CIP-7: Tile Document

TileDocument is a StreamType that stores a mutable JSON document, providing similar functionality as a NoSQL document store.

## Overview

- TileDocuments are streams used for storing JSON data. The TileDocument stream is structured as a single log of [commits](../../../learn/glossary.md#commits), where each commit only contains the diff from the previous version. Optionally, TileDocuments may specify a JSON schema and all commits must adhere to the schema.
- TileDocuments rely on [anchor commits](../../../learn/glossary.md#anchor-commit) for providing immutable timestamps for the [genesis commit](../../../learn/glossary.md#genesis-commit) and subsequent [signed commits](../../../learn/glossary.md#signed-commit) in the stream. In the case of conflicting versions, the branch with the earliest recorded anchor commit will be respected as the canonical branch.
- TileDocuments rely on [DIDs](../../../learn/glossary.md#dids) for [authentication](../../../learn/glossary.md#authentication). Only the DID assigned as the [controller](../../../learn/glossary.md#controllers) of the stream are allowed to perform writes.
- As more updates are made to a single tile, the underlying DAG grows linearly, and so does sync times when fetching the stream over the network. When loading a tile from a node that already has it present, reqponses are very quick
- This guide describes how to create, update, and query TileDocuments using the [JS HTTP Client](../../../build/javascript/installation.md#js-http-client) and the [Core Client](../../../build/javascript/installation.md#js-core-client). You can also interact with TileDocuments from the [CLI](../../../build/cli/installation.md); see the [Quick Start](../../../build/cli/quick-start.md) guide for more information.

## Use-cases

- Store structured (JSON) data
- Data validation using JSON schema
- Determinitic lookup based on metadata
- Historical/specific version access based on commit ID

### Limitations

- Single controller
- No indexing, but metadata will be used, follow conventions
- Growing log, not adapted to frequent updates

## Structure

### Content

- Must be JSON object, not other JSON type (JSON-patch limitation)
- If schema is set in metadata, must pass validation

### Metadata

- `controllers` array but single entry
- `schema` stream or commit ID for validation
- `family` and `tags` for determinism and indexing (future)
  - `family` for network-wide identification ("collection")
  - `tags` for apps/protocols-specific indexing
- `deterministic` flag

## Reference

- CIP
- JS client
