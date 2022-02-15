# **Self.ID SDK**

---

Self.ID is a framework that makes it easy to build Web3 applications with Ethereum-based authentication and composable, user-centric data storage and retrieval.

## **Why Self.ID?**

---

**✅ One library, easy setup**

Self.ID is a single library with minimal configuration that provides access to the full Ceramic stack with support for popular environments such as `React` and `web`. The Self.ID SDK uses [Glaze suite](../glaze/index.md) middleware, [3ID DID](../../docs/advanced/standards/accounts/3id-did.md) accounts, [3ID Connect](../accounts/3id-did.md#3id-connect) authentication, and [Ceramic HTTP client](../core-clients/ceramic-http.md) and [DID JSON-RPC client](../core-clients/did-jsonrpc.md).

**✅ Login with Web3**

Self.ID is compatible with Ethereum accounts and EVM-based wallet authentication, so users don't have to install new wallets or create new accounts in order to use Ceramic. Users can even connect multiple wallet accounts to their Ceramic account, if they like.

**✅ Composable, user-centric data management**

The SDK includes some of the most popular Ceramic data models out-of-the-box, such as [user profiles](https://github.com/ceramicstudio/datamodels/tree/main/packages/identity-profile-basic), linked [crypto accounts](https://github.com/ceramicstudio/datamodels/tree/main/packages/identity-accounts-crypto), and linked [Web2 accounts](https://github.com/ceramicstudio/datamodels/tree/main/packages/identity-accounts-web), giving your application automatic storage and retrieval composability with a rich set of existing users and data to bootstrap your application.

**✅ Extensible data models**

You're not limited to just the data models provided by Self.ID! You can create new data models or import ones from the [Data Models Registry](https://github.com/ceramicstudio/datamodels) to add additional data features to your application.

## **SDK overview**

---

The Self.ID SDK contains modules organized in the following manner:

```
┌─────────────────────────────────────────────┐
│                  framework                  │
├─────────────┬─┬─────────────┬─┬─────────────┤
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

## **Building with React**

---

### [**Using the Framework →**](../../tools/self-id/framework.md)

The Framework is the highest-level abstraction provided by the Self.ID SDK, designed specifically to power [React](https://reactjs.org/) applications. It leverages most other packages of the Self.ID SDK, and in most cases is the module you should use if you're building with React.

<!-- ### [**Using the React module →**]()

The React module is used by the Framework module and provides React-specific components, hooks, and utility functions to help manage user authentication, data storage, and retrieval. Unless you have a specific reason to use this React module, you should consider using the Framework instead. -->

## **Building with JavaScript**

---

### [**Using the Web module →**](../../tools/self-id/write.md)

The Web module provides user authentication, data storage, and retrieval for browser-based web applications.

### [**Using the Core module →**](../../tools/self-id/read.md)

The Core module only provides data retrieval for Node and browser-based applications.
