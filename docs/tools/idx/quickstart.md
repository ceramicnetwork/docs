# Quickstart

Get started exploring what's possible with IDX using the [IDX SDK](https://developers.idx.xyz/reference/idx/){:target="\_blank"}. For usage in a Command Line Interface, you will need to [install the IDX CLI](https://developers.idx.xyz/reference/cli/){:target="\_blank"}.

!!! warning ""

    :octicons-alert-16: IDX is in alpha, Libraries may be unstable and APIs are subject to change. Data created on IDX during alpha will *not* be portable to production. Please share what you're working on and report any issues in the [IDX Discord](https://chat.idx.xyz){:target="_blank"}.

!!! warning ""

    :octicons-stop-16: If you haven't completed the Ceramic quickstart some information here may not make complete sense. We recommend coming here after you've completed the [Ceramic JavaScript Client Quickstart](../../build/javascript/quick-start.md).

## **Prerequisites**

The IDX SDK requires [Node.js](https://nodejs.org/){:target="\_blank"} v14 and npm v6 (usually installed with Node.js). Make sure both are installed.

## **Step 1: Install the IDX SDK**

!!! warning "Authentication"
:octicons-alert-16: If you're using this quickstart anywhere but the Ceramic Playground you'll need to authenticate your Ceramic instance. This is a process that's dependent on your setup so we recommend taking a look at our [authentication section](../../build/javascript/authentication.md){:target="\_blank"} to ensure you don't have any issues following along.

=== "Command"

    ```bash
    npm i @ceramicstudio/idx @ceramicnetwork/http-client
    ```

You will need to connect to a Ceramic node. The following is an example of connecting to the Clay Testnet via the public gateway node hosted by 3BoxLabs.

=== "Command"

    ```JavaScript
    import CeramicClient from '@ceramicnetwork/http-client'
    import { IDX } from '@ceramicstuido/idx'

    const API_URL = 'https://gateway-clay.ceramic.network'

    const ceramic = new CeramicClient(API_URL)

    const idx = new IDX({ceramic})
    ```

## **Step 2: Query a Record**

Let's query a [record](https://developers.idx.xyz/learn/glossary/#record){:target="\_blank"} that stores a [basic profile](https://developers.idx.xyz/guides/definitions/default/#basic-profile){:target="\_blank"}.As you'll see below, we are looking up the user ID: `did:key:z6MkuEd4fm7qNq8hkmWFM1NLVBAXa4t2GcNDdmVzBrRm2DNm`.

=== "Command"

    ```JavaScript
    await idx.get(
        'basicProfile',
        'did:key:z6MkuEd4fm7qNq8hkmWFM1NLVBAXa4t2GcNDdmVzBrRm2DNm'
    )
    ```

=== "Output"

    ```JavaScript
    // Note: this record is live and may not return exactly what you see below.
    {
      name: "Paul",
      residenceCountry: "IE"
    }
    ```

## **Step 3: Create a DID**

The IDX SDK does not support DID Creation, however we've built [3ID-Connect](../../authentication/3id-did/3id-connect.md){:target="\_blank"} to allow web apps to create a DID based off of the user's blockchain wallet.

## **Step 4: Create a Record**

Use the `idx.set()` function to set data to a [record](https://developers.idx.xyz/learn/glossary/#record){:target="\_blank"} for the currently authenticated DID. In this example we're passing the same `basicProfile` alias as above. This will let us record data corresponding to the default basic profile defintion. The second part of the function is the new content we want to set. Finally, the last part of the function is optional but it is the ID that you are trying to set. If nothing is provided it will default to your authenticated DID.

=== "Command"

    ```JavaScript
    await idx.set('basicProfile', {
      name: '<Your Name>'
    })
    ```

## **Step 5: Query Your Record**

Use the `idx.get()` to query your newly created basic profile record.

=== "Command"

    ```JavaScript
    await idx.get('basicProfile')
    ```

=== "Output"

    ```JavaScript
    {
      name: "<Your Name>"
    }
    ```

## **Doing More**

All documentation for the IDX SDK can be [found here](https://developers.idx.xyz/reference/idx/){:target="\_blank"}, for any help or ideas join us on [Discord](https://chat.idx.xyz){:target="\_blank"}!

## **That's It!**

Congratulations on completing this introductory tutorial. You're well on your way to becoming an IDX Developer!
