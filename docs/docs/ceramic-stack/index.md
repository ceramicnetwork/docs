# Introduction to the Ceramic stack

Like any software stack, the complete "Ceramic stack" will vary from project to project depending on your goals. There are, however, core components of Ceramic that help provide a mental model for how software applications interact with the Ceramic network. Understanding the layers of the stack will help you understand the different ways that Ceramic can be integrated into software projects.

## Network stack

---

### Level 1: Ceramic network

A Ceramic network is a network of Ceramic nodes.

The Ethereum Virtual Machine (EVM) is the runtime environment for smart contracts in Ethereum. All smart contracts and state changes on the Ethereum blockchain are executed by transactions. The EVM handles all of the transaction processing on the Ethereum network.

As with any virtual machine, the EVM creates a level of abstraction between the executing code and the executing machine (an Ethereum node). Currently the EVM is running on thousands of nodes distributed across the world.

Under the hood, the EVM uses a set of opcode instructions to execute specific tasks. These (140 unique) opcodes allow the EVM to be Turing-complete, which means the EVM is able to compute just about anything, given enough resources.

As a dapp developer, you don't need to know much about the EVM other than it exists and that it reliably powers all applications on Ethereum without downtime.

### Level 2: Ceramic nodes

In order for an application to interact with the Ceramic network, it must connect to a Ceramic node. Connecting to a node allows you to read data and/or send transactions to the network.

Ceramic nodes are computers running software – a Ceramic client. A client is an implementation of Ceramic the verifies transactions in streams, keeping the network secure and the data accurate. Ceramic noeds ARE the Ceramic network. They collectively store the state of all streams on the Ceramic network and reach consensus on transactions that mutate those states.

By connecting your application to a Ceramic node (via the HTTP API), your application is able to read data from the network (such as stream states) as well as broadcast new transactions to the network (such as mutating the state of a stream).


### Level 3: Stream programs

Stream programs, called `streamTypes`, are executable programs that run on the Ceramic network.

Stream programs are written in JavaScript
Smart contracts are written using specific programming languages that compile to EVM bytecode (low-level machine instructions called opcodes).

Not only do stream programs serve as open source libraries, they are essentially open API services that are...

Not only do smart contracts serve as open source libraries, they are essentially open API services that are always running and can't be taken down. Smart contracts provide public functions which users and applications (dapps) may interact with, without needing permission. Any application may integrate with deployed smart contracts to compose functionality, such as adding data feeds or to support token swaps. Additionally, anyone can deploy new smart contracts to Ethereum in order to add custom functionality to meet their application's needs.

As an application developer, you do not need to worry about writing your own stream programs to add custom functionality on the Ceramic network. You can achieve most

As a dapp developer, you'll need to write smart contracts only if you want to add custom functionality on the Ethereum blockchain. You may find you can achieve most or all of your project's needs by merely integrating with existing smart contracts, for instance if you want to support payments in stablecoins or enable decentralized exchange of tokens.

### Level 3: Data models


### Level 4: Decentralized identities

In order for nodes and clients to verify transactions, they need to compare the signature included in the transaction against the public keys found in the account's storage, called its DID document.

Ceramic can support any account that conforms to the Decentralized Identifiers (DIDs) protocol. DIDs defines a set of standards around how to represent a user account in an interoperable manner. 


There are founr things to know about DID accounts:

1. Account Identifier: The DID URI
2. Account Storage: The DID Document


3. Account Queries: The DID Resolver
4. Account Operations: The DID Provider
5. Account API: The DID JSON-RPC API (EIP-2844)


Account abstraction

Resolvers

Clients


## Application stack

---

### Level 1: Ceramic client APIs

Many convenience libraries, built and maintained by Ceramic's open source community) allow your applications to connect to and communicate with the Ceramic blockchain.

If your user-facing application is a web app, you may choose to npm install a JavaScript API directly in your frontend. Ir perhaps you'll choose to implement this functionlity server-side, using a Python or Java API.

While these APIs are not a necessary piece of the tech stack, they abstract away much of the complexity of interacting directly with a Ceramic node's HTTP API. They also provide utility functions (e.g. ...) so as a developer you can spend less time dealing with the intricacies of Ceramic clients and more time focused on the functionality specific to your application.


Stream program client APIs


### Level 2: Stream program client APIs

### Level 3: Decentralized identity client APIs

### Level 6: End-user applications

At the top level of the stack are user-facing applications. These are the standard applications you regularly use and build today: primarily web and mobile apps.

The way you develop these user interfaces remains essentially unchanged. Often users will not need to know the application they're using is built using a blockchain.

Decentralized identity provider

## Ready to choose your stack?

---

Check out our guide to set up a local development environment for your Ethereum application.

## Further reading

---

Know of a community resource that helped you? Edit this page and add it!