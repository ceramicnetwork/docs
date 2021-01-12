# Pinning
Add and remove documents from the permanent pinset in your Ceramic node.

By default Ceramic will garbage collect any document that has been created or
loaded after some period of time. In order to prevent this you need to pin the
documents that you care about. This will make the Ceramic node keep them around
until they are unpinned again.

## Prerequisites

Pinning requires having [installed a Ceramic client](installation.md) in your
project.

## Add to pinset
Add a document to the permanent pinset using the `pin.add()` method.

``` javascript
const docid = 'kjzl6cwe1jw14...'
await ceramic.pin.add(docid)
```
[:octicons-file-code-16: API reference]()

## Remove from pinset
Remove a document from the permanent pinset using the `pin.rm()` method.

``` javascript
const docid = 'kjzl6cwe1jw14...'
await ceramic.pin.rm(docid)
```
[:octicons-file-code-16: API reference]()

## List documents in pinset
List the documents currently in the permanent pinset using the `pin.ls()`
method.

``` javascript
const docIds = await ceramic.pin.ls()
```
[:octicons-file-code-16: API reference]()



</br>
</br>
</br>
