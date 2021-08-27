# Read-only interactions

The `@self.id/core` package provides APIs for read-only access to records associated to a DID in Node.JS and browser environments.

!!! warning ""

    Before trying to interact with the following APIs, make sure to have a [configured Core instance](configuration.md)

## Using the Core class

A `Core` instance can be used directly to read a record, using the `get()` method with a definition alias as first argument and the DID string as second argument:

```ts
import { Core } from '@self.id/core'

const core = new Core(...)

const profile = await core.get('basicProfile', 'did:3:...')
```

[Core API reference](../../reference/self-id/classes/core.Core.md){: .md-button .md-button }

## Using the PublicID class

For use-cases when it is useful to keep track of a specific DID and possibly reading records multiple times, the `PublicID` class can be used to wrap a DID string:

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

[PublicID API reference](../../reference/self-id/classes/core.PublicID.md){: .md-button .md-button }