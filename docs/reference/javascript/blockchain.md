# Support new blockchains
This guide describes how to add support for new blockchains to the [3ID DID Provider](). Once implemented, users will be able to authenticate their 3ID DID using an account/wallet from that blockchain, and also publicly link this blockchain account to a 3ID DID using a [Caip10Link](../../streamtypes/caip-10-link/overview.md).

Something about the 3ID DID Provider, and also 3ID Connect.

## **Supported blockchains**

| Blockchain | CAIP-2 namespace | Supported providers | Notes |
| -- | -- | -- | -- |
| Cosmos     | [cosmos](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-5.md)    | cosmos provider | The Cosmos wallet provider interface is still being standardized and is subject to change. |
| Ethereum   | [eip155](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-3.md)    | metamask-like ethereum provider |
| Filecoin   | [fil](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-23.md)              | [Filecoin Wallet Provider](https://github.com/openworklabs/filecoin-wallet-provider) |
| EOS        | [eosio](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-7.md)              | [@smontero/eosio-local-provider](https://github.com/sebastianmontero/eosio-local-provider#readme) |
| Polkadot   | [polkadot](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-13.md)              | [@polkadot{.js} extention api](https://polkadot.js.org/) | Doesn't support the `authenticate` method yet. |

## **Requirements**
Ensure your blockchain is represented by a Caip2 
We use [CAIP-10](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) to represent accounts in a blockchain agnostic way. If the blockchain you want to add isn't already part of the CAIP standards you should make sure to add it there.

## 1. Implement AuthProvider
The first step in adding a new blockchain is to create a new [`AuthProvider`](https://github.com/ceramicnetwork/js-ceramic/blob/develop/packages/blockchain-utils-linking/src/auth-provider.ts) class in the [`@ceramicnetwork/blockchain-utils-linking`](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/blockchain-utils-linking) package that implements the [`authenticate()`](#authenticate), [`createLink()`](#createlink), [`accountId()`](#accountid), and [`withAccount()`](#withaccount) functions for your blockchain. After creating the class, add it to the package and export it.

!!! warning ""

    The `AuthProvider` wraps your blockchain provider and is consumed by a [3ID DID Provider]().

### authenticate()
The `authenticate()` function allows a blockchain account to be added as an authentication method (`authMethod`) for a [3ID DID]() by signing a simple message in their blockchain wallet. Behind the scenes, entropy is deterministically generated when the user signs a message with their blockchain account. The seed for the 3ID is encrypted by the `authMethod` and stored in the 3ID's [3ID Keychain]() stream in their [IDX](). When the user signs the same message again with their `AccountID`, they will be able to decrypt and use the seed for their 3ID DID.

Example showing a standard Ethereum Provider (i.e. MetaMask):

``` javascript
.
```

#### Parameters

- `message`: string, can be any string
- `AccountID`: an instance of a CAIP-10 AccountID
- `provider`: specific to your blockchain. This is any standard signer or provider defined for your blockchain. Ideally your ecosystem has a widely-accepted standard interface so that this module can support signing by most accounts.

#### Returns

- `entropy`: a hex string representing 32 bytes of entropy, prefixed by `0x`

!!! warning ""

    The `entropy` returned when a given `AccountID` signs a given `message` will always be the same.

### createLink()
The `createLink()` function enables a blockchain account to generate a verifiable `linkProof` object that binds its AccountID to a 3ID DID. In Ceramic, these `linkProof` objects are stored in [Caip10Link]() streams which allow anyone to look up the 3ID linked to your blockchain account and then resolve any other public information also linked to your DID. 

!!! warning ""

    3ID DID Providers which expose this functionality may optionally store the [streamIDs]() of the Caip10Link streams that belong to a 3ID DID in their [Crypto Accounts](https://developers.idx.xyz/guides/definitions/default/#crypto-accounts) record in [IDX]() for simple lookup.

Example showing a standard Ethereum Provider:
``` javascript
.
```

#### Parameters

This function consumes similar arguments as the [`authenticate()`](#authenticate) function described above. It also consumes the 3ID's [DID string]() that is being linked. This function is implemented such that when a given `AccountID` signs a `message` including a given DID string with the given `provider`, a verifiable `LinkProof` is returned.


### accountId()
The AuthProvider is expected to know which blockchain account it is currently serving. The `accountId()` method should return the user's blockchain account in a CAIP-10 format.

Example showing a standard Ethereum Provider:
``` javascript
.
```

### withAccount()
The `withAccount()` method should return a new instance of the `authProvider` in order to serve a new blockchain account. This allows apps to reuse the same internal settings, such as a connection to a blockchain provider, but with a different account.

Example showing a standard Ethereum Provider:
``` javascript
.
```

## 2. Implement validation
After implementing the AuthProvider, you need to implement validation. Validation checks if _proof-of-ownership_ in the link is formally correct,
i.e. that a well-known payload is actually signed by the account declared in the linkProof.

To implement validation, implement the `BlockchainHandler` interface for your blockchain. It should include a CAIP-2 `namespace` for your blockchain and a `validateLink()` function. When complete, add the newly created `BlockchainHandler` to the `handlers` list in `index.ts` of the [`@ceramicnetwork/blockchain-utils-validation`](https://github.com/ceramicnetwork/js-ceramic/blob/develop/packages/blockchain-utils-validation/src/index.ts) library.


### validateLink()
The `validateLink()` function checks if the signature contained in a `LinkProof` corresponds to the declared account in order to validate a given `LinkProof`. This allows anyone to easily verify `LinkProofs` and for Ceramic to validate [Caip10Links](), which include a `LinkProof`. This function consumes a LinkProof and returns the LinkProof if valid, otherwise it returns null. Valid typically means that the given signature in the LinkProof is valid over the given message and is created by the given account.

Example showing a standard Ethereum Provider:
``` javascript
.
```

!!! warning ""

    Before proceeding, ensure that the `validateLink()` function can validate linkProofs created by the [`createLink()`](#createlink) function.

## 3. Add your blockchain to the proper places

### Multiauth UI
Allow users to see in providers that expose this interface, such as 3ID Connect.

### This page
Supported blockchain list above by making a PR to this page. You can click the edit button found at the top right of this page.




---





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







