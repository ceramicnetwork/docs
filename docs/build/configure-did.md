# Configure your DID
This guide will help you set up a DID instance to your Ceramic client so it can function properly. Ceramic clients require a DID instance and the DID Resolver(s) contained within to verify proper ownership of Ceramic streams by validating signatures on Ceramic Commits when loading a stream.

## Prerequisites

Configuring your DID requires having [installed a Ceramic client](installation.md) in your project.


## Create the DID Resolver
The DID Resolver allows Ceramic to look up information about any DID it encounters within
any Stream it loads or interacts with. It therefore must be capable of resolving more DID
methods than just what is used by the authenticated user. It is recommended that all Ceramic
nodes be able to resolve at least the `did:3` and `did:key` DID methods.

#### Import the resolvers

Import Resolvers for all DID methods that this node will support

``` javascript
import KeyDidResolver from 'key-did-resolver'
import ThreeIdResolver from '@ceramicnetwork/3id-did-resolver'
```

#### Construct a ResolverRegistry of all desired Resolvers

``` javascript
const resolver = { ...KeyDidResolver.getResolver(),
                   ...ThreeIdResolver.getResolver(ceramic) }
```

## Create the DID instance
The DID instance wraps the DID Resolver (and optionally a DID Provider if you intend to [authenticate](authentication.md) your DID to allow writes).

``` javascript
import { DID } from 'dids'
const did = new DID({ resolver })
```

## Set the DID instance on the Ceramic client

Set the DID instance to your Ceramic client so that it can use it to resolve DIDs to validate
ownership of Ceramic Streams.
``` javascript
ceramic.setDID(did)
```

## Authenticate the DID
If you want to be able to perform writes with your Ceramic client, you will need to authenticate your DID first. To do that the DID must be provided a DID Provider instance. More information on how to do this is available on the [authenication](authentication.md) page.

## Usage

After setting the DID instance on the Ceramic client, the application will now be able to perform [reads](queries.md) on Ceramic. If the DID was [authenticated](authentication.md), then the user will also be able to perform [writes](writes.md) on Ceramic using their DID.

</br>
</br>
</br>
