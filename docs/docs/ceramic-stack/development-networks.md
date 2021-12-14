# Development networks

> This page is a stub. You can contribute to making it better by contributing on Github.

When building a Ceramic application with streams, you'll want to run it on a local network to see how it works before deploying it.

Similar to how you might run a local server on your computer for web development, you can use a development network to create a local Ceramic network instance to test your dapp. These Ceramic development networks provide features that allow for much faster iteration than a public testnet (for instance you don’t need to deal with waiting for anchors to process on Ethereum).

## Prerequisites

---

You should understand the basics of the Ethereum stack and Ethereum networks before diving into development networks.

## What is a development network?

---

Development networks are essentially Ethereum clients (implementations of Ethereum) designed specifically for local development.

**Why not just run a standard Ethereum node locally?**

You *could* run a node (like Geth, OpenEthereum, or Nethermind) but since development networks are purpose-built for development, they often come packed with convenient features like:

Deterministically seeding your local blockchain with data (e.g. accounts with ETH balances)
Instantly mining blocks with each transaction it receives, in order and with no delay
Enhanced debugging and logging functionality

## Available tools

---

**Note:** Most development frameworks include a built-in development network. We recommend starting with a framework to set up your local development environment.

### Ganache
Quickly fire up a personal Ethereum blockchain which you can use to run tests, execute commands, and inspect state while controlling how the chain operates.

Ganache provides both a desktop application (Ganache UI), as well as a command-line tool (ganache-cli). It is part of the Truffle suite of tools.

- Website
- GitHub
- Documentation

### Hardhat Network
A local Ethereum network designed for development. It allows you to deploy your contracts, run your tests and debug your code

Hardhat Network comes built-in with Hardhat, an Ethereum development environment for professionals.

Website
GitHub
FURTHER READING
Know of a community resource that helped you? Edit this page and add it!

## Related topics

---

- [Development frameworks ↗]()
- [Set up a local development environment ↗]()