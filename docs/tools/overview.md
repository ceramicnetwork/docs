# Tools

Various tools such as libraries, CLIs and services are created on top of Ceramic to provide abstractions and specific solutions to some common use-cases.

## Glaze

The Glaze umbrella includes projects providing **low-level solutions** on top of Ceramic. Each project focuses on a specific scope, using a restricted number of dependencies on top of Ceramic.

[Glaze overview](glaze/overview.md){: .md-button }

## Self.ID

Self.ID is both a **reference application** and a **SDK** providing higher-level abstractions to create user-centric Web applications. It leverages Glaze projects and [3ID Connect](../reference/accounts/3id-did.md#3id-connect) to offer a simple way to help Web developers get started with the Ceramic ecosystem.

[Self.ID overview](self-id/overview.md){: .md-button }

## IDX

IDX refers to both the [Identity Index specification](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-11/CIP-11.md) and a set of tools (libraries and CLI).

While the specification is still actively used, the IDX SDK and tools are now **deprecated** in favor of Glaze and Self.ID presented above.

[IDX overview](idx/overview.md){: .md-button }

## IdentityLink

IdentityLink is a service that issues verifiable claims which prove that a DID owns one or more Web2 social accounts.

[IdentityLink overview](identitylink/overview.md){: .md-button }
