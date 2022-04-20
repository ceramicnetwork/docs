# **CAIP-10 Link (CIP-7)**

---

CAIP-10 Link (CIP-7) is streamcode that stores a link between a Web3 wallet account and a Ceramic account.

## **About CAIP-10 Link**

---

### **Features**

- **Chain-agnostic** – CAIP-10 Link leverages the [Chain Agnostic Improvement Proposal (CAIP) 10 standard](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md) to represent blockchain accounts from a variety of blockchain networks
- **One-way links** – Links are one way only: from a blockchain account to a Ceramic account (DID). It is not possible to go from a Ceramic account to a blockchain account using only CAIP-10 Links. However, this can be achieved by using the [`identity-accounts-crypto`](https://github.com/ceramicstudio/datamodels/tree/main/models/identity-accounts-crypto) data model which stores a list of CAIP-10 links owned by a Ceramic account.
- **Multi-account** – A Ceramic account (DID) can have an unlimited number of CAIP-10 Links that publicly bind it to many different addresses on many different L1 and L2 blockchain networks.

### **Structure**

- **Content** – The content of a CAIP-10 Link can only store the `string` value of a Ceramic account (DID) or `null` if no DID is linked. Internally, the commits in the CAIP-10 Link contain a signed link proof.
- **Metadata** – The metadata of a CAIP-10 Link is defined by the standard and is not editable by consumers. It contains the blockchain account (CAIP-10 string) as the only entry in `controllers` and adds the chainID as in `family`.

## **Getting started with CAIP-10 Links**

---

### **Available implementations**

Visit the [**JavaScript CAIP-10 Link implementation**](../../../../reference/stream-programs/caip10-link.md) to create and load CAIP-10 Links within your application. Alternatively if you use 3ID Connect for authentication, which comes standard in most Ceramic frameworks such as Self.ID SDK, then it will take care of creating CAIP-10 links for users for you.

## **Further reading**

---

Read the [CAIP-10 Link (CIP-7) standard](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-7/CIP-7.md) for more information about the standard.
