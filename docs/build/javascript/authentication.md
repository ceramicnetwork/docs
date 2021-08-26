# Authentication

This guide will help you set up authentication so your users can perform [writes](./writes.md). Authentication is _not needed_ if you are only [querying](./queries.md) streams.

## **Requirements**

Authentication requires having [installed a Ceramic client](./installation.md). Your client should include the [DID Resolver(s)](../../learn/glossary.md#did-resolver) for the [DID method(s)](../../learn/glossary.md#did-methods) you will use for authentication.

## **1. Choose a DID method**

The first step in adding authentication to your project is choosing which DID method to use for authentication.

[**3ID DID Method**](../../authentication/3id-did/method.md): A powerful DID method that supports multiple keys, key rotations, and revocations

[**Key DID Method**](../../authentication/key-did/method.md): A lightweight DID method that only supports one key and cannot handle rotations

[**NFT DID Method**](../../authentication/nft-did/method.md): A lightweight DID method with permissions that change based on on-chain NFT asset ownership

[**Safe DID Method**](../../authentication/safe-did/method.md) _coming soon_: A lightweight DID method with permissions that change based on on-chain [Gnosis Safe](https://gnosis-safe.io/) contract permissions

!!! warning ""

    It is recommended that most applications use the [3ID DID Method](../../authentication/3id-did/method.md).

## **2. Install a DID Provider**

After choosing a DID method, install a [DID provider](../../learn/glossary.md#did-providers) for that method.

### 3ID DID Providers

#### 3ID Connect

[**3ID Connect**](../../authentication/3id-did/3id-connect.md) is the most popular 3ID DID Provider for Ceramic web apps. The 3ID Connect SDK allows users to authenticate a 3ID DID using their existing blockchain wallets without needing to install any additional software. Developers do not need to worry about DID key management for their users.

[:octicons-download-16: Installation](../../authentication/3id-did/3id-connect.md#installation){: .md-button .md-button--primary }

!!! warning ""

    **Recommended for most browser applications.**

#### 3ID DID Provider

[**3ID DID Provider**](../../authentication/3id-did/provider.md) is a low-level JavaScript 3ID DID Provider. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret.

[:octicons-download-16: Installation](../../authentication/3id-did/provider.md#installation){: .md-button .md-button--primary }

### Key DID Providers

#### Key DID Provider Ed25519

[**Key DID Provider Ed25519**](../../authentication/key-did/provider.md) is a low-level JavaScript Key DID Provider for use with `Ed25519` key pairs. Your application is responsible for key managemet, and users need to authenticate with a DID seed.

[:octicons-download-16: Installation](../../authentication/key-did/provider.md#installation){: .md-button .md-button--primary }
