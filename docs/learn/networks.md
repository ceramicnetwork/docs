# Networks

Networks are collections of Ceramic [nodes](../glossary#nodes) that share specific configurations and communicate over dedicated [libp2p](../glossary#libp2p) topics. Networks are discrete from one another. [Streams](../glossary#streams) that exist on one network are _not_ discoverable on or portable to another.

## Public networks

Ceramic has three public networks that you can use when building applications: Mainnet, Clay Testnet, and Dev Unstable.

### **Mainnet**

Mainnet is the main public network used for production deployments on Ceramic. Ceramic's mainnet nodes communicate over the dedicated `/ceramic/mainnet` libp2p topic and use [Ethereum's](../glossary#ethereum) mainnet blockchain (`EIP155:1`) for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams). Mainnet is currently running and anyone can openly query data from streams on mainnet using the [community mainnet gateway](../run/nodes/community-nodes.md#gateways), but deployments that require writing data to the network are restricted to those projects on the mainnet early launch program (ELP) waitlist. If you want to write data to mainnet, [**sign up for ELP here**](https://blog.ceramic.network/ceramic-mainnet-early-launch-program/). _Over time, we will fully open mainnet to all applications._

### **Clay Testnet**

Clay Testnet is a public Ceramic network used by the community for application prototyping, development, and testing purposes. Ceramic core devs also use Clay for testing official protocol release candidates. While we aim to maintain a high level of quality on the Clay testnet that mirrors the expectations of Mainnet as closely as possible, ultimately the reliability, performance, and stability guarantees of the Clay network are lower than that of Mainnet. Because of this, **the Clay network should not be used for applications in production**. Clay nodes communicate over the dedicated `/ceramic/testnet-clay` libp2p topic and use [Ethereum's](../glossary#ethereum) Rinkeby and Ropsten testnet blockchains for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams). **Clay is openly available for anyone to use now.**

### **Dev Unstable**

Dev Unstable is a public Ceramic network used by Ceramic core protocol developers for testing new protocol features and the most recent commits on the develop branch of `js-ceramic`. It should be considered unstable and highly experimental; only use this network if you want to test the most cutting edge features, but expect issues. Dev Unstable nodes communicate over the dedicated `/ceramic/dev-unstable` libp2p topic and use [Ethereum's](../glossary#ethereum) Rinkeby and Ropsten testnet blockchains for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams).

## Private networks

You can also prototype applications on Ceramic by running the protocol in a local environment completely disconnected from other public nodes.

### **Local**

Local is a private test network used for the local development of Ceramic applications. Nodes connected to the same local network communicate over a randomly-generated libp2p topic `/ceramic/local-$(randomNumber)` and use a local Ethereum blockchain provided by Truffle's Ganache for generating timestamps used in [anchor commits](../glossary#anchor-commit) for [streams](../glossary#streams).
