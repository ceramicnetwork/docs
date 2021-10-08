# Configuration

The Self.ID SDK requires minimum configuration. The only requirement is to provide a [Ceramic endpoint](#ceramic-endpoint), either using a preconfigured option or the URL of a node.

## Ceramic endpoint

The Self.ID `Core` instance creates an internal Ceramic HTTP client connecting to the specified endpoint.

```ts
import { Core } from '@self.id/core'

// connect to a known URL
const core = new Core({ ceramic: 'http://localhost:7007' })
// or use one of the preconfigured option
const core = new Core({ ceramic: 'testnet-clay' }) 
```

This endpoint can be the URL of a known node (`http://localhost:7007` for example), or one of the following preconfigured options:

### `local`

Use this option to connect to your local Ceramic node on `http://localhost:7007`.

!!! warning ""

    Make sure [your local Ceramic node is running](../../build/cli/installation.md#2-start-the-ceramic-node) when using this option.

### `testnet-clay`

Use this option to connect to the Ceramic node provided by 3Box Labs for the Clay testnet.

### `testnet-clay-gateway`

Use this option to connect to the **read-only** Ceramic gateway provided by 3Box Labs for the Clay testnet.

!!! warning ""

    This endpoint can be used by Self.ID packages to read data from the network, but not create or update contents. You should use the `testnet-clay` option for this.

### `mainnet-gateway`

Use this option to connect to the **read-only** Ceramic gateway provided by 3Box Labs for the main network.

!!! warning ""

    This endpoint can be used by Self.ID packages to read data from the network, but not create or update contents. You will need to [join Ceramic's Early Launch Program](https://blog.ceramic.network/ceramic-mainnet-early-launch-program/) to gain write access for your app.

## DataModel

Data interactions with Self.ID are built on top of a [DataModel](../glaze/datamodel.md).

By default, the Self.ID SDK supports interacting with the following models without further configuration necessary:

- `alsoKnownAs`, specified in [CIP-23](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-23/CIP-23.md)
- `basicProfile`, specified in [CIP-19](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md)
- `cryptoAccounts`, specified in [CIP-21](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-21/CIP-21.md)
 
```ts
import { Core } from '@self.id/core'

const core = new Core({ ceramic: 'testnet-clay' })

const profile = await core.get('basicProfile', id)
```

It is possible to provide an alternative model to use in the `Core` instance:

```ts
import { Core } from '@self.id/core'

const model = {
  definitions: {
    profile: 'kjzl6cwe1jw145cjbeko9kil8g9bxszjhyde21ob8epxuxkaon1izyqsu8wgcic',
  },
  schemas: {
    Profile: 'ceramic://k3y52l7qbv1frxt706gqfzmq6cbqdkptzk8uudaryhlkf6ly9vx21hqu4r6k1jqio',
  },
  tiles: {},
}

const core = new Core({ ceramic: 'testnet-clay', model })

const profile = await core.get('profile', id)
```

You can [learn more about creating and using DataModels in the dedicated page](../glaze/datamodel.md). 