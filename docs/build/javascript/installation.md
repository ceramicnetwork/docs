# Choose a client

This page describes the Ceramic clients available to use in your JavaScript project. After choosing either the [JS HTTP Client](#js-http-client) or the [JS Core Client](#js-core-client), follow its installation instructions.

!!! warning "**MAINNET NOW LIVE!**"
    
    We're onboarding the first projects to [Mainnet](../learn/mainnet.md) from the waitlist. If you want to deploy to mainnet in the near future, **[sign up for the Mainnet waitlist](https://blog.ceramic.network/ceramic-mainnet-early-launch-program/)**. 

    While on the waitlist, you can always develop and prototype your integration on the [Clay Testnet](../learn/networks.md#mainnet). Demonstrating a fully-functioning integration that is ready for mainnet is a great way to increase your odds of being seleted from the waitlist. If you have questions or need further prioritization, you can [**join the Discord**](https://chat.ceramic.network) and let us know.


## **JS HTTP Client**

The JS HTTP Client is a lightweight way of interacting with Ceramic. It allows your JavaScript application to connect to a remote Ceramic node over HTTP to [read](../../build/queries.md), [write](../../build/writes.md), and [pin](../../build/pinning.md) streams. The main decision to make when using the JS HTTP Client is which remote Ceramic node to use. 

[:octicons-download-16: Installation](./http.md){: .md-button .md-button--primary }

!!! warning ""

    **The JS HTTP client is recommended when building most applications.**

### Considerations

**Improved performance**: When using the JS HTTP Client, stream processing and validation happens on a remote Ceramic node running on a server, which usually results in improved performance compared to running the full protocol in-browser with the [JS Core Client](./core.md).

**Predictable data availability**: Streams created using the JS HTTP Client can be pinned and made available on a remote Ceramic node which has more uptime and predictable data availability guarantees than, say, running the [JS Core Client](./core.md) directly in-browser where users can open and close tabs causing their streams to come on and offline at unpredictable intervals.

**Some trust in a remote node**: Stream processing and state validation happens on a remote node which the JS HTTP Client trusts. However, it is important to note that user's keys always live client-side and all updates are signed on the JS HTTP Client and then sent to the HTTP endpoint for processing.

**Swap for the JS Core Client at anytime**: The JS HTTP Client and the [JS Core Client](./core.md) implement the same [CeramicApi](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="_blank"} TypeScript interface, so swapping between clients is seamless and doesn't require changing your application logic; it only requires changing your setup.






</br>
</br>
</br>










## **JS Core Client**








| Language | Client | Description | Usage | Details |
| ----- | ------ | ----- | --- | --- |
| JavaScript | HTTP | Allows your project to interact with a remote Ceramic node over HTTP | Most apps (*recommended*) | [Learn and install](../clients/javascript/http.md) |
| JavaScript | Core | Allows your project to run the full Ceramic protocol (API and node) in any JS environment | Tests, fully client side apps, node.js | [Learn and install](../clients/javascript/core.md) |
| JavaScript | CLI | Allows developers to spin up a Ceramic node and/or interact with Ceramic from the command line | Command line, hosting a node | [Learn and install](../clients/javascript/cli.md) |

> For optimal performance and data availability, it is recommended that you use the HTTP Client when building an application.

</br>
</br>
</br>
