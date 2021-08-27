# Community CAS

This page contains a list of free [Ceramic Anchor Services (CAS)](../../learn/glossary.md#anchor-service) hosted by [3Box Labs](https://3boxlabs.com) and made available for the community. The CAS referenced below are hardcoded into every [Ceramic node](../../learn/glossary.md#nodes) and will be automatically used depending on the node's network configuration; no additional setup is needed.

At this time it is not possible to [run your own CAS](./cas.md), however this functionality will be available in the near future.

## **Mainnet CAS**

Nodes connected to [Mainnet](../../learn/networks.md#mainnet) will automatically use this CAS which generates [anchor commits](../../learn/glossary.md#anchor-commit) using the Ethereum Mainnet blockchain (`eip155:1`) and communicates over the `/ceramic/mainnet` libp2p topic.

!!! warning ""

    **Mainnet usage is currently restricted to projects in the Early Launch program (ELP).** Sign up for the ELP waitlist [here](https://blog.ceramic.network/ceramic-mainnet-early-launch-program/).

ETH Mainnet Address:

```
0xaf65E45F4C0BD388F91EeB23cFCd52F4fCdd6Ee2
```

To view Mainnet CAS transactions, see [Etherscan](https://etherscan.io/address/0xaf65e45f4c0bd388f91eeb23cfcd52f4fcdd6ee2).

## **Clay Testnet CAS**

Nodes connected to [Clay Testnet](../../learn/networks.md#clay-testnet) will automatically use this CAS which generates [anchor commits](../../learn/glossary.md#anchor-commit) using the Ethereum Ropsten blockchain and communicates over the `/ceramic/testnet-clay` libp2p topic.

ETH Ropsten Address:

```
0x1C124c86f7fc22e67974337E889a513b16a5703f
```

To view Clay Testnet CAS transactions, see [Etherscan](https://ropsten.etherscan.io/address/0x1C124c86f7fc22e67974337E889a513b16a5703f).

## **Dev Unstable CAS**

Nodes connected to the [Dev Unstable](../../learn/networks.md#dev-unstable) network will use this CAS which generates [anchor commits](../../learn/glossary.md#anchor-commit) using the Ethereum Rinkeby blockchain and communicates over the `/ceramic/dev-unstable` libp2p topic.

ETH Rinkeby Address:

```
0x41Ee0C359D95970A83229D8e9801cc2672390217
```

To view Dev Unstable CAS transactions, see [Etherscan](https://rinkeby.etherscan.io/address/0x41Ee0C359D95970A83229D8e9801cc2672390217).
