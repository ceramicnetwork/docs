# Authentication
This guide will help you add authentication to your project. Authentication is needed to perform [writes](./writes.md). You do not need authentication if you only need to perform [queries](queries.md).

## **Setup**
Authentication requires having [installed a Ceramic client](./installation.md). When setting up your client be sure to include the proper [DID Resolver(s)](../learn/glossary.md#did-resolver) for the DID methods supported by your application.

### 1. Choose a DID method
The first step in adding authentication to your project is choosing which [DID method](../learn/glossary.md#did-methods) to support for accounts. It is recommended that most applications use the [3ID DID Method](../authentication/3id-did/method.md)

[**3ID DID Method**](../authentication/3id-did/method.md) (recommended): A powerful DID method built on Ceramic that supports key rotations and revocations

[**Key DID Method**](../authentication/key-did/method.md): A lightweight but inflexible DID method that does not support key rotations


### 2. Install a DID Provider
After choosing a DID method, install a [DID provider](../learn/glossary.md#did-providers) for that method. It is recommended that most browser applications use [3ID Connect](../authentication/3id-did/3id-connect.md).

#### 3ID DID Providers

[**3ID Connect**](../authentication/3id-did/3id-connect.md): The most popular 3ID provider for Ceramic web apps. The 3ID Connect SDK allows users to control their 3ID DID from their existing blockchain wallets without needing to install any additional software. Developers do not need to worry about DID key management for their users.

[**3ID DID Provider**](../authentication/3id-did/provider.md): A JavaScript library for creating and interacting with 3ID DIDs. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret.

#### Key DID Providers

[**Key DID Provider Ed25519**](../authentication/3id-did/provider.md): A JavaScript library for creating and interacting with Key DIDs for Ed25519 key pairs. Your application is responsible for key managemet, and users need to authenticate with a DID seed.


## **Usage**
After setting up authentication, users will be able to perform [writes](./writes.md) to streams using their DID.

</br>
</br>
</br>
