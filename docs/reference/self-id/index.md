# Self.ID SDK Reference

## What is the Self.ID SDK?

Self.ID is a SDK for building user-centric Web applications using Ceramic and related decentralized technologies.

The Self.ID SDK bundles the [Ceramic HTTP client](../core-clients/ceramic-http.md), [DID client](../core-clients/did-jsonrpc.md) via [3ID Connect](../accounts/3id-did.md#3id-connect) and [Glaze libraries](../glaze/index.md) into a set of higher-level packages.

## Understanding the stack

Self.ID packages are organized in the following stack:

```
┌─────────────────────────────────────────────┐ ┌─────────────┐
│                  framework                  │ │ 3box-legacy │
├─────────────┬─┬─────────────┬─┬─────────────┤ └─────────────┘
├─────────────┤ ├─────────────┤ ├─────────────┤
│    react    │ │  multiauth  │ │ image-utils │
├─────────────┤ ├─────────────┤ └─────────────┘
├─────────────┤ ├─────────────┤
│     web     │ │     ui      │
├─────────────┤ └─────────────┘
├─────────────┤
│    core     │
└─────────────┘
```

### Framework

The [Framework module](modules/framework.md) is the highest-level abstraction provided by the Self.ID SDK, leveraging most of the other packages included in the SDK. It is meant to power [React](https://reactjs.org/) application similar to the [reference self.id application](https://self.id).

### Main packages

The main packages provide a granular set of APIs for user interactions with an [account-based streams index](../../docs/advanced/standards/application-protocols/identity-index.md):

- [Core module](modules/core.md): read-only interactions with the index
- [Web module](modules/web.md): authentication via [3ID Connect](../accounts/3id-did.md#3id-connect), allowing write access in addition to reads to the index
- [React module](modules/react.md): wrapper for the Web module with [React](https://reactjs.org/) APIs

### Utility packages

The utility packages provide specific-purpose APIs to support common use-cases of the framework or applications:

- [UI module](modules/ui.md): shared [React](https://reactjs.org/) components
- [Multiauth module](modules/multiauth.md): wallet authentication flow complementary to [3ID Connect](../accounts/3id-did.md#3id-connect)
- [Image utilities module](modules/image_utils.md): images manipulation, upload and display utilities
- [3Box legacy module](modules/3box_legacy.md): APIs for loading legacy 3Box profiles

## Getting started

The [Tools section](../../tools/self-id/overview.md) provides documentation for getting started using the Self.ID SDK. Make sure to understand the configuration before trying to use the SDK.

[Self.ID configuration](../../tools/self-id/configuration.md){: .md-button }
