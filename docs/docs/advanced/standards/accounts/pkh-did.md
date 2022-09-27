# **PKH DID Accounts**

A Ceramic account that natively supports blockchain accounts. Default usage with did-session allows capabality and session usage, unlocking a powerful auth model with blockchain accounts. 


## **What is a PKH DID account?**

---

A PKH DID account allows interoperability between blockchain accounts and DIDs, allowing a user to directly use their blockchain account as a DID based account to sign, authorize and authenticate. A PKH DID account is entirely controlled by the relating blockchain account, key rotations, changes, deactivation, etc are not supported. 


## **Specification**

---

For the full specification, see [PKH DID Method specification →](https://github.com/w3c-ccg/did-pkh/blob/main/did-pkh-method-draft.md).


### **Account identifier**

```
did:pkh:<CAIP-10>

//With CAIP-10 expansion
did:pkh:<chain_id>:<account_adress>
```

### **DID Document**

PKH DID documents are deterministically generated from their relating blockchain/CAIP-10 account. 

## **Building with PKH DID**

---

To use PKH DIDs for user accounts in your project, install one of the PKH DID Providers below:

### [**Install DID Session →**](../../../../reference/accounts/did-session.md)

**Recommended.** DID-Session is the most popular DID Provider for Ceramic web apps. DID-Session allows developers to use capabilities to permission and manage sessions for a users blockchain account (PKH DID). Sessions allow users to only sign with their blockchain wallets once and then continue to sign Ceramic transactions in their browser for the duration of the session. If you're using the Self.ID SDK, you don't need to worry about installing DID Session, it already includes it.

### [**Install DID Resolver →**](https://github.com/ceramicnetwork/js-ceramic/tree/develop/packages/pkh-did-resolver)

PKH DID Resolver is a JavaScript DID resolver for the PKH DID Method.

## **Additional reading**

---

- [CAIP-10 Account Specification](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-10.md)