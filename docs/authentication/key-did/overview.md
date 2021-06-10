# Key DID

Key DID is one of a few DID methods supported by Ceramic that can be used to perform authenticated writes to streams that require DIDs. This includes the two default StreamTypes: Tile Documents and CAIP-10 Links.

## Usage

See the [Authentication](https://developers.ceramic.network/build/authentication/) page to learn how to use the Key DID method in your Ceramic project.

## Design

The DID document for a Key DID is statically generated from any Ed25519 cryptographic key pair. This Ed25519 key is used to control the DID. Key DID is lightweight, but the main drawback is that its DID document is immutable and has no ability to rotate keys if it is compromised; it is explicitly tied to a single crypto key. This makes Key DIDs best suited for users who will only want to ever use one keypair to control their DID, and who have strong key security practices - such as a developer. 

## **Underlying technologies**

## **License**
Key DID Provider is fully open sourced under MIT and Apache 2.
