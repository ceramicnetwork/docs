# Networks

Ceramic is currently available as four different networks for you to use when building and deploying your application.

## Public Networks

### Mainnet

**Ceramic mainnet** is the main public network used for the production deployment of applications. Ceramic mainnet nodes communicate over the dedicated `/ceramic/mainnet` pubsub topic and are capable of anchoring documents on the Ethereum mainnet blockchain (`EIP155:1`). **Ceramic mainnet is scheduled to go live sometime around Q1 2021**.

### Clay

**Clay** is the main public test network used for the experimental deployment of applications. Clay nodes communicate over the dedicated `/ceramic/testnet-clay` pubsub topic and are capable of anchoring documents on the Ethereum Rinkeby (`EIP155:?`) and Ethereum Ropsten (`EIP155:?`) testnet blockchains. **Clay is available to use now.**

## Development Networks

### Local

**Local** is a private test network used for the local development of applications. Nodes connected to the same local network communicate over a randomly-generated pubsub topic `/ceramic/local-$(randomNumber)` and are capable of anchoring documents on a local Ethereum blockchain provided by Truffle's Ganache (`EIP155:?`).

### In memory

In memory is a private...