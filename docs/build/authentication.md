# Authentication
This guide will help you add authentication to your project. Authentication is needed to perform [writes](writes.md). If you only need to perform [queries](queries.md), you do not need authentication.


## **Prerequisites**
Authentication requires having [installed a Ceramic client](installation.md) in your project and having [configured a DID](configure-did.md) for that client with the proper DID Resolvers.


## Adding authentication


### 1. Choose a DID method
The first step in adding authentication to your project is choosing which *DID method* you want to support for user accounts. Due to their mutability and security, it is recommended that you use 3ID DID for users.

DID Method | Description | Details |
| ------ | ----- | ----- |
| 3ID DID | A complete and flexible DID method built on Ceramic that supports key rotations and revocations | [Learn more](https://github.com/ceramicstudio/js-3id-did-provider){:target="_blank"} |
| Key DID | A lightweight but inflexible DID method that does not support key rotations | [Learn more](https://github.com/ceramicnetwork/key-did-provider-ed25519){:target="_blank"} |


### 2. Install a DID Provider or SDK
After deciding on a DID method, you need to install either a *wallet* or a *provider* for that method. The most commonly used DID providers and wallets can be found below. For most browser applications, it is recommended that you use 3ID Connect.

| Name      | DID Method | Type | Description | Details | Install |
| ----------- | ------ | ---- | ----- | --- | -- |
| 3ID Connect | 3ID DID | Wallet | A hosted wallet and authentication system for browser apps using 3ID DIDs. Your application is not responsible for key management, and users can authenticate with their existing blockchain wallets. | [Learn](https://github.com/ceramicstudio/3id-connect){:target="_blank"} | [Install](https://github.com/ceramicstudio/3id-connect){:target="_blank"} |
| `3id-did-provider` | 3ID DID | Provider | A JavaScript library for creating and interacting with 3ID DIDs. Your application is responsible for key management, and users need to authenticate with a DID seed or an auth secret. | [Learn](https://github.com/ceramicstudio/js-3id-did-provider){:target="_blank"} | [Install](https://github.com/ceramicstudio/3id-connect){:target="_blank"} |
| `key-did-provider-ed25519` | Key DID | Provider | A JavaScript library for creating and interacting with Key DIDs. Your application is responsible for key managemet, and users need to authenticate with a DID seed. | [Learn](https://github.com/ceramicnetwork/key-did-provider-ed25519){:target="_blank"} | [Install](https://github.com/ceramicstudio/3id-connect){:target="_blank"} |


## **Usage**
After setting up authentication, users will now be able to perform [writes](writes.md) to streams using their DID.

</br>
</br>
</br>
