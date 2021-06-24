# JavaScript API References

This page contains a list of all packages and modules included in the Ceramic TypeScript implementation. Click into individual packages for detailed documentation on its APIs and types.

## Node

### Core
[`@ceramicnetwork/core`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_core.html): Implementation of the Ceramic protocol, exposed using a simple JS API.

### Common
[`@ceramicnetwork/common`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_common.html): Common Ceramic types and utilities.

### StreamID
[`@ceramicnetwork/streamid`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_streamid.html): Utility for encoding and decoding StreamIDs and CommitIDs.

### StreamType: Tile Handler
[`@ceramicnetwork/stream-tile-handler`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_stream_tile_handler.html): Handler implementation for the TileDocument StreamType.

### StreamType: Caip10Link Handler
[`@ceramicnetwork/stream-caip10-link-handler`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_stream_caip10_link_handler.html): Handler implementation for the Caip10Link StreamType.

### IPFS Daemon
[`@ceramicnetwork/ipfs-daemon`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_ipfs_daemon.html): JS-IPFS instance with DagJOSE codec.

### IPFS Topology
[`@ceramicnetwork/ipfs-topology`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_ipfs_topology.html): Bootstrap IPFS topology for Ceramic.

### Pinning
[`@ceramicnetwork/pinning-aggregation`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_pinning_aggregation.html): Aggregation of backends for pinning stream content and state.

### Pinning: IPFS Backend
[`@ceramicnetwork/pinning-ipfs-backend`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_pinning_ipfs_backend.html): Backend for pinning IPFS records on IPFS.

### Pinning: Powergate Backend
[`@ceramicnetwork/pinning-powergate-backend`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_pinning_powergate_backend.html): Backend for pinning IPFS records on Filecoin via a Powergate service.

### Logger
[`@ceramicnetwork/logger`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_logger.html): Utilities for logging.

## Clients

### HTTP Client
[`@ceramicnetwork/http-client`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_http_client.html): HTTP client for Ceramic.

### CLI
[`@ceramicnetwork/cli`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_cli.html): Command line interface for Ceramic.

### StreamType: Tile
[`@ceramicnetwork/stream-tile`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_stream_tile.html): Implementation of the TileDocument StreamType.

### StreamType: Caip10Link
[`@ceramicnetwork/stream-caip10-link`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_stream_caip10_link.html): Implementation of the Caip10Link StreamType.

## DIDs

### JS DID
[`js-did`](https://ceramicnetwork.github.io/js-did/classes/did.html): Interface for interacting with DIDs.

### 3ID DID Resolver
[`@ceramicnetwork/3id-did-resolver`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_3id_did_resolver.html): Resolver for the 3ID DID method.

### Key DID Resolver
[`key-did-resolver`](https://developers.ceramic.network/reference/typescript/modules/key_did_resolver.html): Resolver for the Key DID method.

### Blockchain Utils: Linking
[`@ceramicnetwork/blockchain-utils-linking`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_blockchain_utils_linking.html): Utility functions for linking blockchain accounts to DIDs.

### Blockchain Utils: Validation
[`@ceramicnetwork/blockchain-utils-validation`](https://developers.ceramic.network/reference/typescript/modules/_ceramicnetwork_blockchain_utils_validation.html): Utility functions to validate links of blockchain accounts linked a DID
