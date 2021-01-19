# Clients

The JavaScript implementation of the Ceramic Protocol consists of two main packages which can be used at runtime in your project, `@ceramicnetwork/core` and `@ceramicnetwork/http-client`. Both clients implement the same [CeramicApi](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="_blank"} TypeScript interface.

[:octicons-file-code-16: CeramicApi reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.ceramicapi-1.html){:target="_blank"}


## **Core client**
The core client allows you to run the full Ceramic protocol (API and node) directly in any JavaScript environment, including directly in-browser. You might use the Core client if you want your project to be maximally decentralized. However, there are tradeoffs such as performance and data availability since this node can go on and offline as the user opens and closes browser windows.

[:octicons-download-16: Installation](../../build/installation) | [:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_core.ceramic.html){:target="_blank"}


## **HTTP client**
The HTTP clent allows you to interact with a remote Ceramic node from any JavaScript environment. The HTTP client is recommended when building most applications.

[:octicons-download-16: Installation](../../build/installation) | [:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/classes/_ceramicnetwork_http_client.ceramicclient.html){:target="_blank"}

<br />
<br />
<br />
