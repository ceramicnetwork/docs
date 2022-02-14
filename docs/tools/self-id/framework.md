# **Self.ID Framework**

---

`@self.id/framework` is the highest-level abstraction provided by the Self.ID SDK, designed to easily power React applications with React components, hooks, and utility functions for user authentication, data storage, and retrieval.

**Native support for React** – React components, hooks and related utility functions to help manage user authentication, storage, and retrieval on Ceramic.

**Authentication hook** – React hook to easily initiate a Ceramic sign-in flow where users are authenticated client-side with their Ethereum or other EVM-compatible wallet. 

**User data management hook** – Data storage and retrieval APIs are primarily based on the concept of a viewer, aka the *current user* of the app, which can be the currently-authenticated user or the last known authenticated user (for example when using a cookie for persistence). APIs are read-only if the viewer is not authenticated, but support reads and writes if they're authenticated.

## **Getting started with Framework**

---

Visit the [**Self.ID Framework reference →**](../../reference/self-id/modules/framework.md) documentation for full instructions on how to install, configure, and use the module in your application. For convenience, here's a look at what's possible with the Self.ID Framework:

### **Installation**
Install `@self.id/framework` from npm:

``` bash
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

The [`useConnection` hook](../../reference/self-id/modules/framework.md#useconnection) provides a way for applications to access the current authentication state, initiate the authentication flow, and reset the authentication state.

```typescript
import { useConnection } from '@self.id/framework'

function ConnectButton() {
  const [connection, connect, disconnect] = useConnection()

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
      onClick={() => {
        connect()
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

1. A modal prompts the user to select their Web3 wallet
2. Selecting a wallet initiates the connection to access the Ethereum provider
3. An [Ethereum auth provider](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html) is created using the Ethereum provider
4. The auth flow with 3ID Connect starts, using the [Ethereum authentication provider](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html)
5. A [`SelfID` instance](../../reference/self-id/classes/web.SelfID.md) is created and stored in application state

Once this flow is completed, the viewer's cookie is set to the authenticated user and storing data with the user becomes possible.

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