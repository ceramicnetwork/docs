# 3ID Connect

3ID Connect is an authentication SDK for web applications that allows a user to control a [3ID DID](../dids/3id.md) using any number of blockchain accounts/wallets.

3ID Connect allows users to:
- Create a 3ID
- Control their 3ID with one or more blockchain wallets
- 

## **Usage**

See the [**Authentication**](https://developers.ceramic.network/build/authentication/) page to install 3ID Connect into your Ceramic web application.

## **How it works**

### Users without a 3ID linked to their wallet account

1. User arrives at your application and signs in with their blockchain wallet
2. User sees a 3ID Connect account creation prompt asking if they want to create a new 3ID for this account or link to an existing 3ID
3. If user selects "Create New 3ID", they will be prompted in the wallet to sign two messages: one to add this account as an authentication method for their 3ID, and another to create a CAIP-10 Link which publicly associated their account with their 3ID. If the user selects "Link to Existing 3ID", they will be directed to a page where they can select a 3ID from a list of known 3IDs, then they will need to approve the two messages mentioned previously.

### Users with a 3ID linked to their wallet account

1. User arrives at your application and signs in with their blockchain wallet
2. User sees a 3ID Connect permissions prompt asking them to grant your application permissions
3. Once approved, user is authenticated

On future logins, users will not see the permissions prompt again unless they clear local storage for 3ID Connect.

## **Design**

Under the hood 3ID Connect is built on a set of open standards:

- 3ID DID Method
- IDX
- 3ID Keychain
- CAIP-10 Links

## **Supported blockchains**

- Ethereum (and other EVM compatible chains)
- NEAR
- EOS
- Cosmos (coming soon)
- Polkadot (coming soon)

### Adding new blockchains

Follow this [**tutorial**]() to add support for new blockchains to 3ID Connect.
