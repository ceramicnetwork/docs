# Add support for a new blockchain

This document contains a guide on how to add support for a new blockchain to the [`Caip10Link`](../../../streamtypes/caip-10-link/overview) StreamType, and to use it for authentication in Ceramic.

## Ceramic and blockchain accounts

Ceramic interacts with blockchain accounts in two ways: _authentication_ and _linking_.

### Authentication

3ID Connect (using `3id-did-provider`) creates `3id` (Ceramic flavour of DID) private keys
based on an externally-provided entropy. It could be provided by a blockchain account by merely
signing a well-known message. From a user's standpoint,
it is authentication _into_ Ceramic through her blockchain account, be it on Ethereum, Filecoin,
EOS, Cosmos or something else. Same signature (=same entropy) generates same Ceramic DID.

### Linking

In addition to generating a DID a user could also _link_ additional blockchain accounts to a Ceramic DID.
It establishes a relation `blockchain account → DID` that allows one to discover a DID (and associated data like a social profile and data)
based on just a blockchain account. Additionally, a link serves as a proof-of-ownership by DID over the blockchain account.
This is useful for dApp personalization and UX: one sees familiar names instead of `0xgibberish`.

Below one additional process is mentioned: validation. It checks if _proof-of-ownership_ in the link is formally correct,
i.e., a well-known payload is really signed by the account that is declared in the link.

## Adding a new blockchain

To add a new blockchain to Ceramic one has to implement both linking and validation.
We use [CAIP-10](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) to represent accounts in a blockchain agnostic way. If the blockchain you want to add isn't already part of the CAIP standards you should make sure to add it there.

### Linking

To add a new blockchain, one has to implement a new class implementing [AuthProvider](https://github.com/ceramicnetwork/js-ceramic/blob/develop/packages/blockchain-utils-linking/src/auth-provider.ts), put it
into the [`@ceramicnetwork/blockchain-utils-linking`](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/blockchain-utils-linking) package and export it.
The auth provider sits between 3id-connect (or 3ID DID Provider) and your blockchain account provider. In case of Ethereum, it might be MetaMask. It is responsible mainly for:

- authentication (`#authenticate`): provide deterministic entropy
- creating link (`#createLink`): create a LinkProof object which associates the specified AccountID with a DID

The auth provider is expected to know which blockchain account it currently serves. It reports it via `#accountId`.
To reuse the same internal settings, e.g. a connection to a blockchain provider, but with a different account,
the auth provider should have a `#withAddress` method.

Let's look at each method required to be implemented by the **AuthProvider** interface:

#### `authenticate()`

The `authenticate` function allows a blockchain account to be added as an authentication method (authMethod) to a 3ID. This means using your blockchain account you will always be able to access that 3ID and derive its 3ID Keychain for use, for example in 3ID Connect.

##### Parameters

- `message`: string, can be any string
- `AccountID`: an instance of a CAIP-10 AccountID
- `provider`: specific to your blockchain. This is any standard signer or provider defined for your blockchain. Ideally your ecosystem has a widely-accepted standard interface so that this module can support signing by most accounts.

##### Returns

- `entropy`: a hex string representing 32 bytes of entropy, prefixed by `0x`

The entropy returned by a given _AccountID_ must always be the same.

#### `createLink()`

The `createLink` function allows a blockchain account to create a verifiable link proof that publicly binds the blockchain account to a given DID. In Ceramic, these these link proofs can be used to create `CAIP10Link` streams which allow anyone to look up the DID linked to your blockchain account, and then resolve any other public info linked to your DID. The StreamIDs of your CAIP10Links can be stored in the [IDX Crypto Accounts records](https://developers.idx.xyz/guides/definitions/default/#crypto-accounts) for simple lookup.

This function consumes similar arguments as described above. It also consumes the DID string that is being linked. This function is implemented such that when the given AccountID signs a message including the given DID with the given provider, a `LinkProof` is returned.

#### `accountId()`

The `accountId` method should return currently used account in the CAIP-10 format.

#### `withAccount()`

The `withAccount` method should return a new instance of the auth provider that serves a new account.

### Validation

Validation is the counterpart of [linking](#linking) that checks if the signature contained in a `LinkProof` corresponds to the declared account.

To add support for a new blockchain:

- add a new file named after your blockchain to `@ceramicnetwork/blockchain-utils-validation` package
- this file should expose an implementation of the `BlockchainHandler` interface, having:
  - [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) `namespace` for your blockchain
  - a `validateLink` function that checks if the linking signature was created by the account declared in the `LinkProof` argument
- add the newly created `BlockchainHandler` to the `handlers` list in [index.ts](https://github.com/ceramicnetwork/js-ceramic/blob/develop/packages/blockchain-utils-validation/src/index.ts)

#### `validateLink()`

The `validateLink` function validates a given LinkProof. This allows anyone to easily verify LinkProofs and for Ceramic to validate CAIP10Links. The function consumes a LinkProof and returns the LinkProof if valid, otherwise it returns null. Valid typically means that the given signature in the LinkProof is valid over the given message and is created by the given account.

Make sure that `validateLink` can validate links created by `AuthProvider#createLink`.

## Currently supported blockchains

Below you can see a table which lists supported blockchains and their provider objects. If you add support for a new chain, please make a PR here.

| Blockchain | CAIP-2 namespace                                                                | Supported providers                                                                               | Notes                                                                                      |
| ---------- | ------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------ |
| Cosmos     | [cosmos](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-5.md)    | cosmos provider                                                                                   | The Cosmos wallet provider interface is still being standardized and is subject to change. |
| Ethereum   | [eip155](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-3.md)    | metamask-like ethereum provider                                                                   |
| Filecoin   | [fil](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-23.md)      | [Filecoin Wallet Provider](https://github.com/openworklabs/filecoin-wallet-provider)              |
| EOS        | [eosio](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-7.md)     | [@smontero/eosio-local-provider](https://github.com/sebastianmontero/eosio-local-provider#readme) |
| Polkadot   | [polkadot](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-13.md) | [@polkadot{.js} extention api](https://polkadot.js.org/)                                          | Doesn't support the `authenticate` method yet.                                             |
