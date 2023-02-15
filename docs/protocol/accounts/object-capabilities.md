# Object Capabilities

Ceramic streams support [CACAO](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-74.md), allowing a basic but powerful capability-based authorization system to exist. CACAO, or "chain agnostic object capabilities", are composable, transferable and verifiable containers for authorizations and encoded in IPLD.  For the full CACAO specification and more examples, reference [CAIP-74: CACAO - Chain Agnostic CApability Object](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-74.md).

## Approach

---

Object capability-based authorization systems, of which CACAO is an implementation, are a natural way to represent authorizations in open and distributed systems. Object capabilities require little coordination, are only stored by parties that care about a particular capability, and are self-verifiable. 

Contrast this to popular authorization models like access controls lists (ACLs), which often rely on the ability to maintain an accurate, agreed-upon, and up to date list of authorizations. ACLs are simple and sufficient when you can rely on a single authoritative source to maintain the list, but quickly become difficult in a distributed setting. Maintaining a list amongst many unknown participants becomes a difficult consensus problem and often very costly at scale, requiring lots of upfront and continuous coordination. 

## Usage

---

CACAO enables the ability for one account to authorize another account to construct signatures over limited data on their behalf, or in this case write to a Ceramic stream. 

### Using blockchain accounts

When combined with ["Sign-in with X"](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-122.md), CACAO unlocks the ability for blockchain accounts to authorize Ceramic accounts (DIDs) to sign data on their behalf. 

This frequently used pattern in Ceramic greatly increases the the usability of user-owned data and public-key cryptography. Thanks to the adoption of blockchain based systems, many users now have the ability to easily sign data in web-based environments using their wallet and blockchain account. 

### Authorizing sessions

Data-centric systems like Ceramic often have more frequent writes than a blockchain system, so it can be impractical to sign every Ceramic stream event in a blockchain wallet. Instead with the use of CACAO and "Sign-in with X" many writes can be made by way of a temporary key and DID authorized with a CACAO. Allowing a user to only sign once with a blockchain based account and wallet, then continue to sign many payloads for an authorized amount of time (session).

> In the future, we expect the ability to model the permissions and authorizations for more complex environments and structures including full organizations.


## Specification

---

Support for object capabilities in the core Ceramic protocol is described below. Events in streams are signed payloads formatted in IPLD using DAGJWS (DAG-JOSE), as [described here](https://www.notion.so/Event-Log-641d039b8f5e48edb98ea57bba3a64a1). This describes how this is extended to construct a valid signed payload using CACAO in DAGJWS, by example of first constructing a JWS with CACAO. A JWS with CACAO can then be directly encoded with DAG-JOSE after. 

### JWS with CACAO

JWS CACAO support includes adding a `cap` parameter to the JWS Protected Header and specifying the correct `kid` parameter string. Here is an example protected JWS header with CACAO:

```tsx
{ 
  "alg": "EdDSA",
  "cap": "ipfs://bafyreidoaclgf2ptbvflwalfrr6d4iqehkzyidwbzaouprdbjjfb4yim6q"
  "kid": "did:key:z6MkrBdNdwUPnXDVD1DCxedzVVBpaGi8aSmoXFAeKNgtAer8#z6MkrBdNdwUPnXDVD1DCxedzVVBpaGi8aSmoXFAeKNgtAer8"
 }
```

Where:

- **`alg`** - identifies the cryptographic algorithm used to secure the JWS
- **`cap`** - maps to a URI string, expected to be an IPLD CID resolvable to a CACAO object
- **`kid`** - references the key used to secure the JWS. In the scope here this is expected to be a DID with reference to any key in the DID verification methods. The parameter MUST match the `aud` target of the CACAO object for both the CACAO and corresponding signature to be valid together.


> Since `cap` is currently not a registered header parameter name in the IANA "JSON Web Signature and Encryption Header Parameters" registry, we treat this as a "Private Header Parameter Name" for now with additional meaning provided by the CACAO for implementations that choose to use this specification.

>This means that ignoring the `cap` header during validation will still result in a valid JWS payload by the key defined in the `kid`. It just has no additional meaning by what is defined in the CACAO. The `cap` header parameter could also have support added as an extension by using the `crit` (Critical) Header Parameter in the JWS, but there is little reason to invalidate the JWS based on a consumer not understanding the `cap` header given it is still valid.

### DagJWS with CACAO

**Construction**

Given JWS with CACAO described in prior section, follow the DAG-JOSE specification and implementations for the steps to construct a given JWS with CACAO header and payload into a DagJWS. DagJWS is very similar to any JWS, except that the payload is a base64url encoded IPLD CID that references the JSON object payload.

```tsx
{ 
  cid: "bagcqcera2mews3mbbzs...quxj4bes7fujkms4kxhvqem2a",
  value: {
    jws: { 
      link: CID("bafyreidkjgg6bi4juwx...lb2usana7jvnmtyjb4xbgwl6e"),
      payload: "AXESIGpJjeCjiaWv...LKw6pIDQfTVrJ4SHlwmsvx", 
      signatures: [
        {
          protected: "eyJhbGciOiJFZERTQSIsImNh...GU2djZEpLTmhYSDl4Rm9rdEFKaXlIQiJ9"
          signature: "6usTYvu5KN0LFTQsWE9U-tqx...h60EgfvjL_rlAW7_tnQUl84sQyogpkLAQ"
        }
      ]
    }
  }
}
```

**Verification**

The following algorithm describes the steps required to determine if a given DagJWS with CACAO is valid:

1. Follow [DAG-JOSE specification](https://ipld.io/specs/codecs/dag-jose/spec/) to transform a given DagJWS into a JWS.
2. Follow JWS specifications to determine if the given JWS is valid. Verifying that the given signature paired with `alg` and `kid` in the protected header is valid over the given payload. If invalid, an error MUST be raised.
3. Resolve the given URI in `cap` parameter of the projected JWS header to a CACAO JSON object. Follow the [CAIP-74 CACAO](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-74.md) specification to determine if the given CACAO is valid. If invalid, an error MUST be raised.
4. Ensure that the `aud` parameter of the CACAO payload is the same target as the `kid` parameter in the JWS protected header. If they do not match, an error MUST be raised.

**Example**

Example IPLD dag-jose encoded block, strings abbreviated.

```tsx
{ 
  cid: "bagcqcera2mews3mbbzs...quxj4bes7fujkms4kxhvqem2a",
  value: {
    jws: { 
      link: CID("bafyreidkjgg6bi4juwx...lb2usana7jvnmtyjb4xbgwl6e"),
      payload: "AXESIGpJjeCjiaWv...LKw6pIDQfTVrJ4SHlwmsvx", 
      signatures: [
        {
          protected: "eyJhbGciOiJFZERTQSIsImNh...GU2djZEpLTmhYSDl4Rm9rdEFKaXlIQiJ9"
          signature: "6usTYvu5KN0LFTQsWE9U-tqx...h60EgfvjL_rlAW7_tnQUl84sQyogpkLAQ"
        }
      ]
    }
  }
}
```

If `block.value.jws.signatures[0].protected` is decoded, you would see the following object, a JWS protected header as described above:

```tsx
{
  "alg": "EdDSA",
  "cap": "ipfs://bafyreidoaclgf...yidwbzaouprdbjjfb4yim6q",
  "kid": "did:key:z6Mkq2ZyjGV54ev...hXH9xFoktAJiyHB#z6Mkq2ZyjGV54ev...hXH9xFoktAJiyHB"
}
```