# Hosting an anchor service

This guide describes how to spin up and run a hosted [Ceramic Anchor Service (CAS)](../../learn/glossary.md#anchor-service) in JavaScript that can be used to generate [anchor commits](../../learn/glossary.md#anchor-commit) for streams from a Ceramic node.

!!! warning ""

    It is currently not possible to run your own anchor service. See hosted options below.
    
## **Hosted options**

### Mainnet

### Clay Testnet
    
Ceramic network has few separate networks: `dev-unstable`, `testnet-clay`, and `mainnet`. 3Box Labs hosts an instance of Ceramic Service for each network.
`dev-unstable` and `testnet-clay` use Ethereum Rinkeby or Ethereum Ropsten for anchoring. Transactions are free here, so the anchor services are freely available.
`mainnet` anchor service on the other hand uses main Ethereum network. For some time the mainnet anchor service is only available to early-launch partners because of DoS concerns.
When Ceramic network operates in a _fully_ decentralized way, one could run own mainnet anchor service.
For now one should rely on the hosted anchor services only.

For more details about the JavaScript CAS implementation, refer to its repo:

[:octicons-file-code-16: Ceramic Anchor Service](https://github.com/ceramicnetwork/ceramic-anchor-service){:target="_blank"}
