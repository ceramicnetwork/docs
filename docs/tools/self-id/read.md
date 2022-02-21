# **Self.ID Core**

The `@self.id/core` module provides read-only user data APIs for Node and browser-based applications.

> If you need user data storage, use [Self.ID Framework](framework.md) for React or [Self.ID Web](write.md) for web.

## **Getting started with Self.ID Core**

---

Visit the [**Self.ID Core reference →**](../../reference/self-id/modules/core.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with Self.ID Core:

### **Installation**

Install `@self.id/core` from npm:

```bash
npm install @self.id/core
```

### **Setup and configuration**

Before using the Self.ID Core APIs, make sure you have [configured your setup](configuration.md) including your node and data models.

#### Connect to a Ceramic node

The Self.ID `Core` instance generates a Ceramic client that needs to connect to a Ceramic node:

```ts
import { Core } from '@self.id/core'

// connect to a known URL
const core = new Core({ ceramic: 'http://localhost:7007' })
// or use one of the preconfigured option
const core = new Core({ ceramic: 'testnet-clay' })
```

This Ceramic endpoint can be the URL of any known node, `http://localhost:7007`, or one of Self.ID's [preconfigured node options](configuration.md#connect-to-a-ceramic-node).

#### Configure data models

Self.ID core is [pre-configured with a few popular data models](configuration.md#configure-your-data-models) that you can use to retrieve data from the network. You can read about those data models, and learn how to add additional data models, in the [data models configuration](configuration.md#using-additional-data-models) page.

### **Retrieving user data**

#### Using the core instance

The [Self.ID `Core`](../../reference/self-id/classes/core.Core.md) instance can be used directly to read user data from Ceramic, using the `get()` method with a data model (definition) alias as the first argument and the user's Ceramic account as the second argument:

```ts
import { Core } from '@self.id/core'

const core = new Core(...)

const profile = await core.get('basicProfile', 'did:3:...')
```

#### Using the PublicID class

For use-cases when it is helpful to keep track of a specific account and possibly retrieve its data multiple times, the [`PublicID`](../../reference/self-id/classes/core.PublicID.md) class can be used to wrap a Ceramic account:

```ts
import { Core, PublicID } from '@self.id/core'

const core = new Core(...)
const aliceID = 'did:3:123...'

// rather than using the Core instance directly as followed...
const [profile, accounts] = await Promise.all([
  core.get('basicProfile', aliceID),
  core.get('cryptoAccounts', aliceID),
])

// ... a PublicID instance can be used:
const alice = new PublicID({ core, id: aliceID })
const [profile, accounts] = await Promise.all([
  alice.get('basicProfile'),
  alice.get('cryptoAccounts'),
])
```
