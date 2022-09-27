# **Self.ID Framework**

---

`@self.id/framework` is the highest-level abstraction provided by the Self.ID SDK, designed to easily power React applications with React components, hooks, and utility functions for user authentication, data storage, and retrieval.

**Native support for React** – React components, hooks and related utility functions to help manage user authentication, storage, and retrieval on Ceramic.

**Authentication hook** – React hook to easily initiate a Ceramic sign-in flow where users are authenticated client-side with their Ethereum or other EVM-compatible wallet.

**User data management hook** – Data storage and retrieval APIs are primarily based on the concept of a viewer, aka the _current user_ of the app, which can be the currently-authenticated user or the last known authenticated user (for example when using a cookie for persistence). APIs are read-only if the viewer is not authenticated, but support reads and writes if they're authenticated.

## **Getting started with Framework**

---

Visit the [**Self.ID Framework reference →**](../../reference/self-id/modules/framework.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with the Self.ID Framework:

### **Installation**

Install `@self.id/framework` from npm:

```bash
npm install @self.id/framework
```

### **Setup and configuration**

The [`Provider` component](../../reference/self-id/modules/framework.md#provider) must be added at the root of the application tree in order to use the React hooks described below. It can be used to provide a custom configuration for the Self.ID clients, authentication, state and UI options.

```typescript
import { Provider } from '@self.id/framework'

function App({ children }) {
  return <Provider client={{ ceramic: 'testnet-clay' }}>{children}</Provider>
}
```

### **User authentication**

The [`useViewerConnection` hook](../../reference/self-id/modules/framework.md#useviewerconnection) provides a way for applications to access the current authentication state, initiate the authentication flow, and reset the authentication state.

```typescript
import { useViewerConnection } from '@self.id/framework'

function ConnectButton() {
  const [connection, connect, disconnect] = useViewerConnection()

  return connection.status === 'connected' ? (
    <button
      onClick={() => {
        disconnect()
      }}>
      Disconnect ({connection.selfID.id})
    </button>
  ) : 'ethereum' in window ? (
    <button
      disabled={connection.status === 'connecting'}
      onClick={async () => {
        const accounts = await window.ethereum.request({
          method: 'eth_requestAccounts',
        })
        await connect(new EthereumAuthProvider(window.ethereum, accounts[0]))
      }}>
      Connect
    </button>
  ) : (
    <p>
      An injected Ethereum provider such as{' '}
      <a href="https://metamask.io/">MetaMask</a> is needed to authenticate.
    </p>
  )
}
```

The user authentication flow consists of the following steps:

1. An [Ethereum authentication provider](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html) is created using the Ethereum provider
2. The auth flow with 3ID Connect starts, using the [Ethereum authentication provider](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html)
3. A [`SelfID` instance](../../reference/self-id/classes/web.SelfID.md) is created and stored in application state

Once this flow is completed, the viewer's cookie is set to the authenticated user and storing data with the user becomes possible.

### **Auth Session Management**

Reference [did-session](../../learn/reference/accounts/did-session.md) for more examples of managing the session for a user. Following code expands on example above. 

```ts
// ...
const [connection, connect, disconnect] = useViewerConnection()
// ...
// get session string you serialized and stored before, check if still valid (or how much longer)
const sessionStr = ...
const selfid = await connect(new EthereumAuthProvider(window.ethereum, accounts[0]), sessionStr)
// ...
// get session to serialize and store 
const session = selfid.client.session //or connection.selfID.client.session
session.serialize()
// ...
```

### **Data management**

The [`useViewerRecord` hook](../../reference/self-id/modules/framework.md#useviewerrecord) loads the viewer's data for a given data model (definition), with the following variants:

- If no viewer is set, no data can be loaded
- If the viewer is not authenticated, the data gets loaded but cannot be modified
- If the viewer is authenticated, the data gets loaded and can be modified

```typescript
import { useViewerRecord } from '@self.id/framework'

function ShowViewerName() {
  const record = useViewerRecord('basicProfile')

  const text = record.isLoading
    ? 'Loading...'
    : record.content
    ? `Hello ${record.content.name || 'stranger'}`
    : 'No profile to load'
  return <p>{text}</p>
}

function SetViewerName() {
  const record = useViewerRecord('basicProfile')

  return (
    <button
      disabled={!record.isMutable || record.isMutating}
      onClick={async () => {
        await record.merge({ name: 'Alice' })
      }}>
      Set name
    </button>
  )
}
```

The [`usePublicRecord` hook](../../reference/self-id/modules/framework.md#usepublicrecord) is similar to the `useViewerRecord` hook described above, but is used to read public data from an explicitly provided account rather than the viewer. This hook is read-only and can be used, for example, to retrieve the public data for other users of your application to display in a UI.

```typescript
import { usePublicRecord } from '@self.id/framework'

function ShowProfileName({ did }) {
  const record = usePublicRecord('basicProfile', did)

  const text = record.isLoading
    ? 'Loading...'
    : record.content
    ? `Hello ${record.content.name || 'stranger'}`
    : 'No profile to load'
  return <p>{text}</p>
}
```
### **Upgrading from 0.3.x to 0.4.x**

Version `0.4.x` switched the default authentication method and libray from [3id-connect](../../reference/accounts/3id-did.md) with [3ID DIDs](../../docs/advanced/standards/accounts/3id-did.md) to [did-session](../../learn/reference/accounts/did-session.md) with [PKH DIDs](../../docs/advanced/standards/accounts/pkh-did.md). If you wish to upgrade and still use 3id-connect you can pass a flag and configure your provider as follows. There are no other changes in `v0.4.x`, making upgrading not required at the moment if you dont wish too change auth methods, but PKH DIDs will be the recommended account going forward. 

```typescript
import { Provider } from '@self.id/framework'

function App({ children }) {
  return <Provider client={{ ceramic: 'testnet-clay' }} threeidConnect={true}>{children}</Provider>
}
```

!!! warning ""

    **Switching authentication methods with out consideration will change DIDs for users and result in any prior data not being resolved. **

## **Advanced**

---

### **Server-side rendering**

Server-side rendering can be used to improve the user experience for the first load of an app or page.
The Self.ID Framework exports a [`RequestClient` class](../../reference/self-id/classes/react.RequestClient.md) that can be used to fetch data on a server in order to have it immediately available to the `usePublicRecord` and `useViewerRecord` hooks.

This example shows how server-side rendering can be used in a [Next.js](https://nextjs.org/) page, using the `ShowViewerName` component from the `useViewerRecord` hook example:

```typescript
import { Provider, RequestClient } from '@self.id/framework'

export const getServerSideProps = async (ctx) => {
  const client = new RequestClient({
    ceramic: 'testnet-clay',
    // Inject the cookie from the request headers to parse the viewerID
    cookie: ctx.req.headers.cookie,
  })
  if (client.viewerID != null) {
    // If the viewerID is set, fetch its profile
    await client.prefetch('basicProfile', client.viewerID)
  }
  return { props: { state: client.getState() } }
}

// Use the state prop injected by the server
export default function Home({ state }) {
  return (
    <Provider state={state}>
      <ShowViewerName />
    </Provider>
  )
}
```
