# Pinning
Pinning allows you to add and remove streams from the permanent pinset in your Ceramic node.

!!! warning ""
    By default Ceramic will garbage collect any stream that has been created, modified, or loaded after some period of time. In order to prevent the loss of streams due to garbage collection, you need to pin the streams that you wish to persist. Pinning instructs the Ceramic node to keep them around in persistent storage until they are unpinned.

## Prerequisites

Pinning requires having [installed a Ceramic client](installation.md) in your
project.

## Add to pinset
Use the [`pin.add()`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.pinapi-1.html#add){:target="_blank"} method to add a stream to your permanent pinset.

``` javascript
const streamId = 'kjzl6cwe1jw14...'
await ceramic.pin.add(streamId)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.pinapi-1.html#add){:target="_blank"}

## Remove from pinset
Use the [`pin.rm()`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.pinapi-1.html#rm){:target="_blank"} method to remove a stream from your permanent pinset.

``` javascript
const streamId = 'kjzl6cwe1jw14...'
await ceramic.pin.rm(streamId)
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.pinapi-1.html#rm){:target="_blank"}

## List streamss in pinset
Use the [`pin.ls()`](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.pinapi-1.html#ls){:target="_blank"} method to list streams currently in your permanent pinset.

``` javascript
const streamIds = await ceramic.pin.ls()
```

[:octicons-file-code-16: API reference](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.pinapi-1.html#ls){:target="_blank"}

</br>
</br>
</br>
