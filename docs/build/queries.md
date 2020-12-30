# Queries
Query documents on the Ceramic network.

## Prerequisites
You need to have an [installed client]() to perform queries.

## Query a single document
Use the `method()` method to load a single document.

=== "HTTP"

    ``` javascript
    const doc = await client.createDocument('doctypeName', { content: { foo: bar } })
    ```

    [:octicons-file-code-16: HTTP reference]()

=== "JS"

    ``` javascript
    const doc = await ceramic.createDocument('doctypeName', { content: { foo: bar } })
    ```

    [:octicons-file-code-16: JS reference]()

=== "CLI"

    ``` javascript
    const doc = await client.createDocument('doctypeName', { content: { foo: bar } })
    ```

    [:octicons-file-code-16: CLI reference]()

## Query multiple documents
Use the `method()` method to load multiple documents at once.

=== "HTTP"

    ``` javascript
    const doc = await client.createDocument('doctypeName', { content: { foo: bar } })
    ```

    [:octicons-file-code-16: HTTP reference]()

=== "JS"

    ``` javascript
    const doc = await ceramic.createDocument('doctypeName', { content: { foo: bar } })
    ```

    [:octicons-file-code-16: JS reference]()

=== "CLI"

    ``` javascript
    const doc = await client.createDocument('doctypeName', { content: { foo: bar } })
    ```

    [:octicons-file-code-16: CLI reference]()

## Query a document path
Use the `method()` method to load a document using a path through linked documents.

=== "HTTP"

    ``` javascript
    await client.loadDocument()
    ```

    [:octicons-file-code-16: HTTP reference]()

=== "JS"

    ``` javascript
    await ceramic.loadDocument()
    ```

    [:octicons-file-code-16: JS reference]()

=== "CLI"

    ``` javascript
    await ceramic.loadDocument()
    ```

    [:octicons-file-code-16: CLI reference]()

## Additional Resources
For more API methods, view the [API reference]().