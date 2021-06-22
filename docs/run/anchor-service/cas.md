# Hosting an anchor service

This guide describes how to spin up and run a hosted [Ceramic Anchor Service (CAS)](../../learn/glossary.md#anchor-service) in JavaScript that generates [anchor commits](../../learn/glossary.md#anchor-commit) for streams in batches. You can find the JavaScript CAS implementation [here](https://github.com/ceramicnetwork/ceramic-anchor-service){:target="_blank"}.

!!! warning ""

    It is currently not possible to run your own CAS. Once Ceramic opens mainnet to the public, this will be possible.
    
## **Usage**

By default, every Ceramic node automatically uses a CAS provided by the community based on the node's network configuration. You do not need any additional setup. In the future, nodes will be able to specify which CAS they use.


## **Community CAS**
Free CAS services are offered by [3Box Labs](https://3boxlabs.com) for Ceramic developers.

### Mainnet
For nodes connected to [Mainnet](../../learn/networks.md#mainnet), they will use a Mainnet CAS which generates anchor records using the Ethereum Mainnet blockchain (`eip155:1`) and communicates over the `/ceramic/mainnet` libp2p topic.

### Clay Testnet
For nodes connected to [Clay Testnet](../../learn/networks.md#clay-testnet), they will use a Clay Testnet CAS which generates anchor records using the Ethereum Rinkeby or Ropsten blockchains and communicates over the `/ceramic/testnet-clay` libp2p topic.

### Dev Unstable
For nodes connected to [Dev Unstable](../../learn/networks.md#dev-unstable), they will use a Dev Unstable CAS which generates anchor records using the Ethereum Rinkeby or Ropsten blockchains and communicates over the `/ceramic/dev-unstable` libp2p topic.

</br></br></br>
