# Choose a JS client

This page describes the Ceramic clients available to use in your JavaScript project. After choosing either the [JS HTTP Client](#js-http-client) or the [JS Core Client](#js-core-client), follow its installation instructions.  

## **JS HTTP Client**

The JS HTTP Client is a lightweight way of interacting with Ceramic. It allows your JavaScript application to connect to a remote Ceramic node over HTTP to [read](./queries.md), [write](./writes.md), and [pin](./pinning.md) streams. The main decision to make when using the JS HTTP Client is which remote Ceramic node to use.

[:octicons-download-16: Installation](./http.md){: .md-button .md-button--primary }

!!! warning ""

    **The JS HTTP client is recommended when building most applications.**

### Considerations

**Improved performance**: When using the JS HTTP Client, stream processing and validation happens on a remote Ceramic node running on a server, which usually results in improved performance compared to running the full protocol with the [JS Core Client](#js-core-client).

**Predictable data availability**: Streams created using the JS HTTP Client can be pinned and made available on a remote Ceramic node which has high uptime and predictable data availability guarantees.

**Some trust in a remote node**: Stream processing and state validation happens on a remote node which the JS HTTP Client trusts. However, it is important to note that user's keys always live client-side and all updates are signed on the JS HTTP Client and then sent to the HTTP endpoint for processing.

**Swap for the JS Core Client at anytime**: The JS HTTP Client and the [JS Core Client](#js-core-client) implement the same [CeramicApi](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="\_blank"} TypeScript interface, so swapping between clients is seamless and doesn't require changing your application logic; it only requires changing your setup.

## **JS Core Client**

The JS Core Client allows you to run the full Ceramic protocol (client and node) directly in JavaScript environments such as in your tests or in node.js. Most applications instead use the [JS HTTP Client](#js-http-client).

### Considerations

**Maximal security and decentralization**: The Ceramic Core client does not have trusted relationships with any external nodes. With Ceramic Core, streams that are [written](./writes.md), [queried](./queries.md), or [pinned](./pinning.md) are verified in the local environment which is great if you need maximal security and decentralization in your application.

**Transitory data availability**: Streams created with Ceramic Core will only be available on the network as long as this node remains online. For more resilient data availability you can always replicate and pin streams on secondary long-running nodes, or instead use the [JS HTTP Client](#js-http-client) which relies on a remote node more likely to always be online.

**Setup complexity**: You will need to configure an [IPFS](../../learn/glossary.md#ipfs) node which supports the [dag-jose](../../learn/glossary.md#dagjose) data format and ensure connectivity to the rest of the Ceramic network.

**Swap for JS HTTP Client at any time**: The JS Core Client and the [JS HTTP Client](#js-http-client) implement the same [CeramicApi](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="\_blank"} TypeScript interface, so swapping between clients is seamless and doesn't require changing your application logic; it only requires changing your setup.
