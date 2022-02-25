# **Self.ID SDK configuration**

---

The Self.ID SDK requires minimum configuration. In this guide you will learn how to:

- Connect the SDK to a Ceramic node
- Configure your data models

## **Connect to a Ceramic node**

---

In all Self.ID modules, you'll need to specify which Ceramic node to use. This endpoint can be the URL of any known Ceramic node, `http://localhost:7007`, or one of the following preconfigured options:

| Preconfigurations | Network | Permissions | Implementation | Host |
| ----- | ----- | ----- | ----- | ----- |
| `local` | Local | Read/Write | JS Ceramic | Local |
| `testnet-clay` | Clay testnet | Read/Write | JS Ceramic | 3Box Labs |
| `testnet-clay-gateway` | Clay testnet | Read-only | JS Ceramic | 3Box Labs |
| `mainnet-gateway` | Mainnet | Read-only | JS Ceramic | 3Box Labs |

!!! warning ""

    When using localhost, ensure your local Ceramic node [is running](../../build/cli/installation.md#2-start-the-ceramic-node)
    

!!! warning ""
    
    The mainnet configuration for Self.ID is read-only. To perform writes on mainnet, you will need to join Ceramic's Mainnet [Early Launch Program](https://blog.ceramic.network/ceramic-mainnet-early-launch-program/).


## **Configure your data models**

---

Self.ID APIs are built on top of data models. By default, the SDK supports interacting with the following data models without further configuration:

| Data model | Alias | Description |
| ----- | ----- | ----- |
| `identity-profile-basic` ([CIP-19](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md))| `basicProfile` | Stores a user's profile |
| `identity-accounts-web` ([CIP-23](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md)) | `alsoKnownAs` | Stores verifiable credentials that link a user's Web2 accounts to their Ceramic account. |
| `identity-accounts-crypto` ([CIP-21](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md)) | `cryptoAccounts` | Stores a list of CAIP-10 Links that link a user's Web3 accounts to their Ceramic account. |


```ts
import { Core } from '@self.id/core'

const core = new Core({ ceramic: 'testnet-clay' })

const profile = await core.get('basicProfile', id)
```

### **Using additional data models**

It is possible to provide alternative models for use in the Self.ID `Core` module, or any Self.ID module that extends it, including `Web`, `React`, and `Framework`. 

```ts
import { Core } from '@self.id/core'

const model = {
  definitions: {
    profile: 'kjzl6cwe1jw145cjbeko9kil8g9bxszjhyde21ob8epxuxkaon1izyqsu8wgcic',
  },
  schemas: {
    Profile:
      'ceramic://k3y52l7qbv1frxt706gqfzmq6cbqdkptzk8uudaryhlkf6ly9vx21hqu4r6k1jqio',
  },
  tiles: {},
}

const core = new Core({ ceramic: 'testnet-clay', model })

const profile = await core.get('profile', id)
```

!!! warning ""

    You can [learn more about creating and using DataModels in the dedicated page](../glaze/datamodel.md).
