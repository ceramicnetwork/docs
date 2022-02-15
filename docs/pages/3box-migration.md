# 3ID Migration: 3Box to Ceramic

If you have used 3Box with 3ID-Connect in the past, you will already have an existing 3ID. No additional steps have to be taken to migrate your existing 3ID to the Ceramic network. When you interact with any application on Ceramic through 3ID-Connect, your 3ID will automatically be migrated.

The migration includes moving your 3ID and control of your 3ID to the Ceramic network, migrating your 3Box profile to the profile definition and schema in IDX, and lastly migrating your address, Twitter and Github links.

Any additional data in 3Box may or may not be migrated by the applications themselves built on 3Box. Those applications will guide you through any additional migration steps if necessary.

## **Migration Difficulties**

Most users will be able to migrate without difficulty, but there is a number of known instances where we can not easily migrate your 3ID. In theses cases we will create a new 3ID for you and partially migrate any data that we can. You will be able to re-add any profile data and social links that fail to migrate in the future.

**Very Early 3Box Users**

Early 3Box users will have muport DIDs instead of the now standard 3ID DID implementation. For future interoperability in Ceramic and to take advantage of all the features in 3ID we have chosen not to migrate these DIDs. When you use 3ID-Connect on Ceramic we will detect that you have a muport DID and instead create a new 3ID for you. We will still migrate your profile data if we can, but will not migrate your Twitter and Github links.

**Multiple Linked Accounts**

There was a known past bug in 3Box that resulted in multiple addresses being linked to a DID in an unexpected way. When you use 3ID-Connect on Ceramic we will detect that you have one of these accounts and instead create a new 3ID for you. Users accounts that have this issue may not have expected to link these accounts, so we will not migrate your profile data or your social links.

**Migration Failures**

3Box existed for a while before Ceramic and we may have not built support for all prior existing formats. If migration fails at any point during the process, we will still attempt partial migrations when we can and continue with a best effort migration. If migration fails during 3ID migration, we will create a new 3ID and try to migrate your profile data still. If migration fails during profile or social link migration, we will return your migrated 3ID anyways.

## **For Developers**

You can find more details in this [blog post](https://blog.ceramic.network/migrating-from-3box-to-ceramic-and-idx/), if you are interested in the more technical details of the migration or in migrating your own application from 3Box to Ceramic.

## **Questions or support?**

We're always available to answer any questions and help you through this migration. Reach out to us in the [Ceramic Discord](https://chat.ceramic.network/) for assistance.
