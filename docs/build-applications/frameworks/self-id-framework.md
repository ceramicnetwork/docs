---
hide:
  - toc
---

# Self.ID Framework

Self.ID is development framework that bundles everything you need to build a user-facing Ceramic application with practically zero-configuration.

![](../../../images/self-id.png)

[Get started with a tutorial](){: .md-button .md-button--primary }

### The Self.ID Framework
Self.ID includes many of the most popular Ceramic developer tools in one easy-to-use SDK.

| Name | Description |
| ----------- | ----------- |
| Ceramic.js       | Ceramic client |
| TileDocument.js | Tile program client |
| CAIP10Link.js | CAIP-10 Link program client |
| DID DataStore.js        | IDX protocol client |
| DataModels.js        | Data models runtime |
| Self.ID Connect | Hosted 3ID DID provider compatible with blockchain wallets. Keep writing a longer description because realistically there should be enough content here for someone to really understand. |

It also includes utilities for [managing decentralized images]().

### Example usage
This snippet shows an application using the Self.ID Core SDK to access a Ceramic node running on the Clay testnet, then querying for the "basicProfile" stored in did:3:...'s DID DataStore.

```ts
import { Core } from '@self.id/core'

const core = new Core({ ceramic: 'testnet-clay' })

const profile = await core.get('basicProfile', 'did:3:...')
```

### Learn more about Self.ID SDKs
Use platform-specific libraries to include and work with the Self.ID stack in your application.

<div class="txtl-options">
  <a href="./hub/" class="box">
    <h5>React  →</h5>
  </a>
  <span class="box-space"> </span>
  <a href="./hub/apis" class="box">
    <h5>Web + Node.js →</h5>
  </a>
  <span class="box-space"> </span>
  <a href="./tutorials/hub/web-app/" class="box">
    <h5>Next →</h5>
  </a>
</div>


