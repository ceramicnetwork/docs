# Authentication

This guide will help you set up authentication so your users can perform [writes](./writes.md). Authentication is _not needed_ if you are only [querying](./queries.md) streams.

## **Requirements**

Authentication requires having [installed a Ceramic client](./installation.md). Your client should include the [DID Resolver(s)](../../learn/glossary.md#did-resolver) for the [DID method(s)](../../learn/glossary.md#did-methods) you will use for authentication.

## **1. Choose a DID method**

The first step in adding authentication to your project is choosing which DID method to use for authentication.

[**PKH DID Method**](../../docs/advanced/standards/accounts/3id-did.md): A DID method that natively supports blockchain accounts, default usage with did-session allows capabality and session usage.

[**3ID DID Method**](../../docs/advanced/standards/accounts/3id-did.md): A powerful DID method that supports multiple keys, key rotations, and revocations

[**Key DID Method**](../../docs/advanced/standards/accounts/key-did.md): A lightweight DID method that only supports one key and cannot handle rotations

[**NFT DID Method**](../../docs/advanced/standards/accounts/nft-did.md): A lightweight DID method with permissions that change based on on-chain NFT asset ownership

[**Safe DID Method**](../../docs/advanced/standards/accounts/safe-did.md) _coming soon_: A lightweight DID method with permissions that change based on on-chain [Gnosis Safe](https://gnosis-safe.io/) contract permissions

!!! warning ""

    It is recommended that most applications use the [PKH DID Method with DID Session](../../docs/advanced/standards/accounts/pkh-did.md).

## **2. Install a DID Provider**

After choosing a DID method, install a [DID provider](../../learn/glossary.md#did-providers) for that method.

### PKH DID Providers

#### DID Session

[**DID Session**](../../reference/accounts/did-session.md) is the most popular DID Provider for Ceramic web apps. DID-Session allows developers to use capabilities to permission and manage sessions for a users blockchain account (PKH DID). Sessions allow users to only sign with their blockchain wallets once and then continue to sign Ceramic transactions in their browser for the duration of the session. 

[:octicons-download-16: Installation](../../reference/accounts/did-session.md){: .md-button .md-button--primary }

!!! warning ""

    **Recommended for most browser applications.**


### 3ID DID Providers

#### 3ID Connect

[**3ID Connect**](../../docs/advanced/standards/accounts/3id-did.md#3id-connect) SDK allows users to authenticate a 3ID DID using their existing blockchain wallets without needing to install any additional software. Developers do not need to worry about DID key management for their users. For most use cases it is now suggested to use did-session over 3id-connect. 

[:octicons-download-16: Installation](../../reference/accounts/3id-did.md#3id-connect){: .md-button .md-button--primary }

#### 3ID DID Provider

[**3ID DID Provider**](../../docs/advanced/standards/accounts/3id-did.md#3id-did-provider) is a low-level JavaScript 3ID DID Provider. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret.

[:octicons-download-16: Installation](../../reference/accounts/3id-did.md#3id-did-provider){: .md-button .md-button--primary }

### Key DID Providers

#### Key DID Provider Ed25519

[**Key DID Provider Ed25519**](../../docs/advanced/standards/accounts/key-did.md#key-did-provider-ed25519) is a low-level JavaScript Key DID Provider for use with `Ed25519` key pairs. Your application is responsible for key managemet, and users need to authenticate with a DID seed.

[:octicons-download-16: Installation](../../reference/accounts/key-did.md#ed25519){: .md-button .md-button--primary }
