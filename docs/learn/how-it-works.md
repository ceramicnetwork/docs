# **How it works**

---

Ceramic decentralizes application databases, making data universally composable and reusable across applications. The network consists of three core parts: a highly-scalable, decentralized infrastructure for data availability and consensus, a marketplace of community-created data models, and a suite of standard APIs for storing, updating, and retrieving data from those models.

## **Core components**

---

The Ceramic network consists of three core components:

1. [Scalable, decentralized data infrastructure](#scalable-decentralized-data-infrastructure)
2. [Data models marketplace](#data-models-marketplace)
3. [Open APIs for data storage, update, and retrieval](#open-apis)

## **Scalable, decentralized data infrastructure**

---

The most foundational layer of Ceramic is its scalable, decentralized data network. The Ceramic network consists of a collection of permissionless nodes that work together to provide data availability for all states stored on the network, and work to come to consensus about those states every time there is a new transaction.

However, unlike Layer 1 blockchains designed to keep track of state for financial applications such as tokens, Ceramic is specifically designed to keep track of state for high-throughput data applications such as decentralized social networks, decentralized identity, crypto gaming, reputation systems, etc. In this way, Ceramic acts as a global, highly-scalable decentralized database that every application in the world can build on.

To achieve scale, Ceramic makes a few opinionated decisions on the data structure of its network. The most important is that in Ceramic, there is no notion of state that can be shared between accounts. Every piece of state is owned only by the account that created it, and no account can modify anyone else’s states, though any account can link to a piece of state owned by another account.

On Ceramic, every account has a collection of mutable data objects, called streams, that only they as the owner of those streams, can write to. The content stored in each stream is arbitrary, and can reference content in anyone else’s stream. Note that this does not preclude compute. Developers can write functions, called streamcode, that define how these streams can be updated and what actions they perform upon each new update.

This architecture untangles state between users, allowing the system to scale horizontally very cleanly. You can imagine that accounts 1 - 1,000,000 are replicated on one set of Ceramic nodes, and accounts 1,000,001 - 2,000,000 are replicated on another. Theoretically, the network can be sharded all the way down to each individual user if needed without breaking composability. In order to ensure state verifiability and composability between user shards, Ceramic relies on a merkle tree data structure that aggregates transactions across all users, allowing any any account to verify the integrity of anyone else’s streams at any time.

## **Data models marketplace**

---

The second core component of Ceramic is its vibrant ecosystem of open source data models created by the community, which serve to unlock cross-application data composability. Data models are a novel abstraction that unify how similar types of applications store and retrieve state from each individual user on the network. For example, you can imagine that every decentralized Twitter implementation would run on a few shared data models: one for each user’s tweets, one for their social graph, one for their DMs, etc. By adopting the same underlying data models, applications are able to natively interoperate on the same data.

> You can compare Ceramic’s use of data model standards to the use of token standards for financial ledgers. On Ethereum, for example, the introduction of the ERC-20 fungible token and ERC-721 non-fungible token standards have given rise to entire ecosystems of tokens and financial applications that natively interoperate. Ceramic brings this same concept to data.

Ceramic takes a community-driven approach to creating these data models, allowing any developer to easily define, share, and reuse their models with other developers in the ecosystem. As more data models are created by the community, we will see a continuous expansion in the quantity and variety of applications that are built with composable data.

Composability done this way also makes the developer experience better. Building an application on Ceramic looks like browsing a marketplace of data models, plugging them into your app, and automatically gaining access to all data on the network that’s stored in these models. No longer will every single developer need to worry about bootstrapping their application with their own siloed users and data, making it easier than ever to go from idea to implementation.

## **Open APIs**

---

The final core component of Ceramic is its permissionless, open APIs for storing, modifying, and retrieving data from the network. By standardizing, generalizing, and opening these APIs up to every developer in the world, Ceramic enables developers to build on top of shared resources stored on the network without fear of centralization, censorship risk, or lock-in.
