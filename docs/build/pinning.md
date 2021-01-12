# Pinning
Pinning allows you to add and remove documents from the permanent pinset in your Ceramic node.

!!! note "" 
    By default Ceramic will garbage collect any document that has been created, modified, or loaded after some period of time. In order to prevent the loss of documents due to garbage collection, you need to pin the documents that you wish to persist. Pinning instructs the Ceramic node to keep them around until they are unpinned.

## Prerequisites

Pinning requires having [installed a Ceramic client](installation.md) in your
project.

## Add to pinset
Use the [`pin.add()`]() method to add a document to your permanent pinset.

``` javascript
const docid = 'kjzl6cwe1jw14...'
await ceramic.pin.add(docid)
```

[:octicons-file-code-16: API reference]()

## Remove from pinset
Use the [`pin.rm()`]() method to remove a document from your permanent pinset.

``` javascript
const docid = 'kjzl6cwe1jw14...'
await ceramic.pin.rm(docid)
```

[:octicons-file-code-16: API reference]()

## List documents in pinset
Use the [`pin.ls()`]() method to list documents currently in your permanent pinset.

``` javascript
const docIds = await ceramic.pin.ls()
```

[:octicons-file-code-16: API reference]()

</br>
</br>
</br>
