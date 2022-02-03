# Self.ID Framework

The [`@self.id/framework` package](../../reference/self-id/modules/framework.md) provides a React-based framework for building [Self.ID](overview.md#applications) and similar applications.

It combines other packages of the Self.ID SDK in the following stack:

```txt
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

## Framework stack

The framework leverages most of the other packages of the Self.ID SDK, such as [`@self.id/core`](../../reference/self-id/modules/core.md) and [`@self.id/web`](../../reference/self-id/modules/web.md) at the lowest levels. However, it is unlikely developers need to interact directly with these APIs, as higher-level ones are provided.

### React

The [`@self.id/react` package](../../reference/self-id/modules/react.md) used by the framework provides React components, hooks and related utility functions to help manage authentication and interactions with records, similar to [`@self.id/web`](../../reference/self-id/modules/web.md) but designed specifically to be used with React.

It is possible to use the [`@self.id/react` package](../../reference/self-id/modules/react.md) directly if you don't need the full set of framework features.

### UI Components

A shared theme and React components used by the framework and other packages are provided by the [`@self.id/ui` package](../../reference/self-id/modules/ui.md).

This package can be used independently of the framework, however it is specifically designed to support the needs of other Self.ID packages and apps rather being a generic implementation.

### Wallet to DID authentication

Ceramic and by extension Self.ID leverage DIDs as "user accounts" used to interact with data records, therefore an important part of most applications logic requires authentication to access such DIDs.

The [`@self.id/web`](../../reference/self-id/modules/web.md) package leverages [3ID Connect](../../reference/accounts/3id-did.md#3id-connect) to access a DID using an Ethereum authentication provider, while the [`@self.id/multiauth` package](../../reference/self-id/modules/multiauth.md) provides a React-based interface to handle the connection flow to an Ethereum Wallet.

By combining APIs from these two packages, the framework provides a unified authentication flow described below.

## Viewer management

Many interactions provided by the framework are based on the concept of a "viewer". The Viewer can be considered as the "current user" of the app, for which data records are loaded.

With Self.ID, users can only be authenticated client-side (in browser) via [3ID Connect](../../reference/accounts/3id-did.md#3id-connect), so the Viewer can represent the currently authenticated user (DID) of the app, or the last known authenticated DID, for example when using a cookie for persistence.

The framework provides APIs to interact with the Viewer's records for reads only if the Viewer is not authenticated and for writes as well if authenticated.

## Authentication flow

The framework provides a React hook to easily initiate an authentication flow for the Viewer. This flow is made of the following steps:

1. A modal prompts the user to select a Wallet to connect with
1. Selecting a Wallet initiates the connection to access the Ethereum provider
1. An [Ethereum authentication provider](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html) is created using the Ethereum provider
1. The authentication flow with 3ID Connect starts, using the [Ethereum authentication provider](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_blockchain_utils_linking.ethereumauthprovider-1.html)
1. A [`SelfID` instance](../../reference/self-id/classes/web.SelfID.md) is created and stored in the application state

Once this flow is successfully applied, the Viewer's cookie is set to the authenticated DID and writing records associated to the Viewer becomes possible.

## Main APIs

### `Provider` component

The [`Provider` component](../../reference/self-id/modules/framework.md#provider) must be added at the root of the application tree in order to use the hooks described below.
It can be used to provide a custom configuration for the Self.ID clients, authentication, state and UI options.

```typescript
import { Provider } from '@self.id/framework'

function App({ children }) {
  return <Provider client={{ ceramic: 'testnet-clay' }}>{children}</Provider>
}
```

### `useConnection` hook

The [`useConnection` hook](../../reference/self-id/modules/framework.md#useconnection) provides a way for applications to access the current connection state, initiate the connection flow and reset the connection state.

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

### `useViewerRecord` hook

The [`useViewerRecord` hook](../../reference/self-id/modules/framework.md#useviewerrecord) loads the record for a given definition in the index of the current viewer, with the following variants:

- If no viewer is set, no record can be loaded
- If the viewer is not authenticated, the record gets loaded but cannot be mutated
- If the viewer is authenticated, the record gets loaded and be mutated

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

### `usePublicRecord` hook

The [`usePublicRecord` hook](../../reference/self-id/modules/framework.md#usepublicrecord) is similar to the `useViewerRecord` hook described above, but reading from the index of an explicitly provided account rather than the viewer. Public records are read-only, `useViewerRecord` must be used in case mutations are needed.

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

[Framework API reference](../../reference/self-id/modules/framework.md){: .md-button .md-button }

## Advanced features

### Server-side prefetching

Server-side rendering can be used to improve the user experience for the first load of an app or page.
The framework exports a [`RequestClient` class](../../reference/self-id/classes/react.RequestClient.md) from the `@self.id/react` package that can be used to fetch wanted records on the server in order to have them immediately available by the `usePublicRecord` and `useViewerRecord` hooks.

The following example shows how this can be used in a [Next.js](https://nextjs.org/) page, using the `ShowViewerName` component from the `useViewerRecord` hook example:

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

[RequestClient API reference](../../reference/self-id/classes/react.RequestClient.md){: .md-button .md-button }
