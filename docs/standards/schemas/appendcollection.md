# AppendCollection

AppendCollection is a set of schemas for breaking up a large list (a collection) into multiple Ceramic streams.

See [CIP-85](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-85/CIP-85.md) for the complete AppendCollection schema specification.

## **Description**

Storing a list of items in a single Ceramic stream can make the stream grow large in size, possibly exceeding Ceramic's internal limits. AppendCollection schemas present a way to store a possibly large list of items split into multiple streams based on two complementary schemas: `AppendCollection` and `CollectionSlice`. This allows to create a virtually infinite list of items such as a feed by leveraging multiple Ceramic streams rather than a single one.

## **Schemas**

Two schemas are needed to represent a collection: the `AppendCollection` schema represents an entry point to the collection, and the `CollectionSlice` schema represents a single slice of the full list of items.

A Collection "instance" would therefore be made of a single `AppendCollection` stream and any number of `CollectionSlice` streams with cross-references.

### AppendCollection

The `AppendCollection` schema must be an `object` with a `$ceramic` field from [CIP-88](../CIP-88/CIP-88.md) having the type `appendCollection` and pointing to the slice's `schema`, along with the following properties:

- `sliceMaxItems`: the maximum number of items a single slice should contain
- `slicesCount`: the total number of slices the collection contains

```js
{
  $schema: 'http://json-schema.org/draft-07/schema#',
  $ceramic: { type: 'appendCollection', sliceSchema: '<slice schema ID>' },
  title: 'MyCollection',
  type: 'object',
  properties: {
    sliceMaxItems: { type: 'integer', minimum: 10, maximum: 256 },
    slicesCount: { type: 'integer', minimum: 1 },
  },
  required: ['sliceMaxItems', 'slicesCount']
}
```

### CollectionSlice

The `CollectionSlice` schema must be an `object` with a `$ceramic` field from [CIP-88](../CIP-88/CIP-88.md) having the type `collectionSlice` and pointing to the slice's `schema`, along with the following properties:

- `collection`: the StreamID of the collection the slice is part of
- `sliceIndex`: index of the slice in the collection, between `0` and the collection's `slicesCount` minus `1`
- `contents`: array with a `maxItems` value matching the `AppendCollection` `sliceMaxItems` property and defining the items schemas, that must include `{ type: 'null' }` in order to support removals

```js
{
  $schema: 'http://json-schema.org/draft-07/schema#',
  $ceramic: { type: 'collectionSlice' },
  title: 'MyCollectionSlice',
  type: 'object',
  properties: {
    collection: { type: 'string', maxLength: 150 },
    sliceIndex: { type: 'integer', minimum: 0 },
    contents: {
      type: 'array',
      maxItems: 256,
      minItems: 0,
      items: {
        oneOf: [{ type: 'object', ... }, { type: 'null' }],
      },
    },
  },
  required: ['collection', 'sliceIndex', 'contents'],
}
```

## **Usage**

AppendCollection is not yet implemented, however it can be prioritized if needed by the community. Please see [AppendCollection (CIP-85)](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-85/CIP-85.md) for more detail required for implementation such as algorithms.
