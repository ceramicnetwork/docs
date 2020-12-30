# Ed25519 Key DID Method
The Ed25519 Key DID method is a lightweight [DID method]() that uses an Ed25519 key as a management key which cannot be rotated.


## Overview

Unlike other DID methods which are registered and published Ed25519

- DID document: immutable
- Authenticate with only a seed

## How it works

## Recommended usage
Since the DID document is immutable and the management key cannot be rotated, it is recommended that the Ed25519 DID method be used for cases where the owner of the DID is ...

## Implementations

### Providers and Wallets

#### Providers

##### Ed25519 Key DID Provider

#### Wallets

There are currently no wallet implementations that support Ed25519 Key DID provider.

### Resolvers
The [DID resolver]() for the Ed25519 Key DID method is `key-did-resolver-ed25519`.

## Ceramic support
`key-did-resolver-ed25519` is included in the JavaScript implementation of Ceramic by default and so Key DIDs are supported out of the box.

The [Ceramic CLI]() defaults to using the Ed25519 Key DID method since this is primarily a developer interface and it is assumed that developers have a good way of managing secrets. However, other DID methods supported by Ceramic can also be used by the CLI.
