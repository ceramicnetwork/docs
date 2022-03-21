# **Ceramic HTTP API**

---

The Ceramic HTTP API is the standard lowest-level communication protocol between clients and nodes on the Ceramic network. It allows client applications to manually make REST HTTP requests to a remote Ceramic node to send transactons, retrieve data, and "pin" data to make it available.

If you are building an application, you will usually interact with Ceramic using a client API, such as the [JS HTTP Client](../javascript/installation.md#js-http-client), or a framework such as the [Self.ID SDK](../frameworks/index.md), which includes the JS HTTP client by default.

## **When to use the HTTP API**

---

The HTTP API is useful if you have a special use case where you directly want to make manual HTTP requests, or you want to implement an HTTP client in a new language.

!!! warning "Gateway mode"

    Some HTTP API methods will not be available if the Ceramic node you are using runs in *gateway mode*. This option disables writes, which is useful when exposing your node to the internet. **API methods that are disabled when running in gateway mode will be clearly marked.**

## **Streams API**

The `stream` endpoint is used to create new streams and load streams from the node using a StreamID or genesis content.

### Loading a stream

Load the state of a stream given its StreamID.

=== "Request"

    ```
    GET /api/v0/streams/:streamid
    ```

    Here, `:streamid` should be replaced by the StreamID of the stream that is being requested.

=== "Response"
The response body contains the following fields:

    - `streamId` - the StreamID of the requested stream as string
    - `state` - the state of the requested stream as [StreamState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.streamstate-1.html){:target="_blank"}

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/streams/kjzl6cwe1jw147r7878h32yazawcll6bxe5v92348cxitif6cota91qp68grbhm
    ```

=== "Response"

    ```bash
    {
      "streamId": "kjzl6cwe1jw147r7878h32yazawcll6bxe5v92348cxitif6cota91qp68grbhm",
      "state": {
        "type": 0,
        "content": {
          "Ceramic": "pottery"
        },
        "metadata": {
          "schema": null,
          "controllers": [
            "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
          ]
        },
        "signature": 2,
        "anchorStatus": "PENDING",
        "log": [{
          "cid": "bagcqceramof2xi7kh6qblirzkbc7yulcjcticlcob6uvdrx3bexgks37ilva",
          "type": 0
        }],
        "anchorScheduledFor": "12/15/2020, 2:45:00 PM"
      }
    }
    ```

### Creating stream

**:octicons-alert-16: Disabled in gateway mode**

Create a new stream, or load a stream from its genesis content. The genesis content may be signed (e.g. DagJWS for the [TileDocument StreamType](../../docs/advanced/standards/stream-programs/tile-document.md)), or unsigned in some cases.

=== "Request"

    ```bash
    POST /api/v0/streams
    ```

    #### Request body fields:

    - `type` - the type code of the StreamType to use (e.g. `0` for TileDocuments). Type codes for the supported stream types can be found [in this table](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-59/tables/streamtypes.csv).
    - `genesis` - the genesis content of the stream (will differ per StreamType)
    - `opts` - options for the stream creation, [CreateOpts](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.createopts-1.html){:target="_blank"} (optional)

=== "Response"

    The response body contains the following fields:

    - `streamId` - the StreamID of the requested stream as string
    - `state` - the state of the requested stream as [StreamState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.streamstate-1.html){:target="_blank"}

#### Example

This example creates a `TileDocument` from an unsigned genesis commit. Note that if the content is defined for a `TileDocument` genesis commit, it needs to be signed.

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/streams -X POST -d '{
        "type": 0,
        "genesis": {
          "header": {
            "family": "test",
            "controllers": ["did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"]
          }
        }
      }' -H "Content-Type: application/json"
    ```

=== "Response"

    ```bash
    {
      "streamId": "k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl",
      "state": {
        "type": 0,
        "content": {},
        "metadata": {
          "family": "test",
          "controllers": [
            "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
          ]
        },
        "signature": 0,
        "anchorStatus": "PENDING",
        "log": [
          {
            "cid": "bafyreihtdxfb6cpcvomm2c2elm3re2onqaix6frq4nbg45eaqszh5mifre",
            "type": 0
          }
        ],
        "anchorScheduledFor": "12/15/2020, 3:00:00 PM"
      }
    }
    ```

## **Multiqueries API**

The `multiqueries` endpoint enables querying multiple streams at once, as well as querying streams which are linked.

### Querying multiple streams

This endpoint allows you to query multiple StreamIDs. Along with each StreamID an array of paths can be passed. If any of the paths within the stream structure contains a Ceramic StreamID url (`ceramic://<StreamID>`), this linked stream will also be returned as part of the response.

=== "Request"

    ```bash
    POST /api/v0/multiqueries
    ```

    #### Request body fields:
    - `queries` - an array of [MultiQuery](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.multiquery-1.html){:target="_blank"} objects

=== "Response"

    The response body contains a map from StreamID strings to [StreamState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.streamstate-1.html){:target="_blank"} objects.

#### Example

First let's create three streams to query using the ceramic cli:

=== "Request1"

    ```bash
    ceramic create tile --content '{ "Document": "A" }'
    ```

=== "Response1"

    ```bash
    StreamID(kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc)
    {
      "Document": "A"
    }
    ```

=== "Request2"

    ```bash
    ceramic create tile --content '{ "Document": "B" }'
    ```

=== "Response2"

    ```bash
    StreamID(kjzl6cwe1jw147w3xz3xrcd37chh2rz4dfra3imtnsni385rfyqa3hbx42qwal0)
    {
      "Document": "B"
    }
    ```

=== "Request3"

    ```bash
    ceramic create tile --content '{
        "Document": "C",
        "link": "ceramic://kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc"
    }'
    ```

=== "Response3"

    ```bash
    StreamID(kjzl6cwe1jw14b54pb10voc4bqh73qyu8o6cfu66hoi3feidbbj81i5rohh7kgl)
    {
      "link": "ceramic://kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc",
      "Document": "C"
    }
    ```

Now let's query them though the multiqueries endpoint:

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/multiqueries -X POST -d '{
      "queries": [{
        "streamId": "kjzl6cwe1jw14b54pb10voc4bqh73qyu8o6cfu66hoi3feidbbj81i5rohh7kgl",
        "paths": ["link"]
      }, {
        "streamId": "kjzl6cwe1jw147w3xz3xrcd37chh2rz4dfra3imtnsni385rfyqa3hbx42qwal0",
        "paths": []
      }]
    }' -H "Content-Type: application/json"
    ```

=== "Response"

    ```bash
    {
      "kjzl6cwe1jw14b54pb10voc4bqh73qyu8o6cfu66hoi3feidbbj81i5rohh7kgl": {
        "type": 0,
        "content": {
          "link": "ceramic://kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc",
          "Document": "C"
        },
        "metadata": {
          "schema": null,
          "controllers": [
            "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
          ]
        },
        "signature": 2,
        "anchorStatus": "PENDING",
        "log": [
          {
            "cid": "bagcqcera5nx45nccxvjjyxsq3so5po77kpqzbfsydy6yflnkt6p5tnjvhbkq",
            "type": 0
          }
        ],
        "anchorScheduledFor": "12/30/2020, 1:45:00 PM"
      },
      "kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc": {
        "type": 0,
        "content": {
          "Document": "A"
        },
        "metadata": {
          "schema": null,
          "controllers": [
            "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
          ]
        },
        "signature": 2,
        "anchorStatus": "PENDING",
        "log": [
          {
            "cid": "bagcqcerawq5h7otlkdwuai7vhogqhs2aeaauwbu2aqclrh4iyu5h54qqogma",
            "type": 0
          }
        ],
        "anchorScheduledFor": "12/30/2020, 1:45:00 PM"
      },
      "kjzl6cwe1jw147w3xz3xrcd37chh2rz4dfra3imtnsni385rfyqa3hbx42qwal0": {
        "type": 0,
        "content": {
          "Document": "B"
        },
        "metadata": {
          "schema": null,
          "controllers": [
            "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
          ]
        },
        "signature": 2,
        "anchorStatus": "PENDING",
        "log": [
          {
            "cid": "bagcqceranecdjzw4xheudgkr2amjkntpktci2xv44d7v4hbft3ndpptid6ka",
            "type": 0
          }
        ],
        "anchorScheduledFor": "12/30/2020, 1:45:00 PM"
      }
    }
    ```

## **Commits API**

The `commits` endpoint provides lower level access to the data structure of a Ceramic stream. It is also the enpoint that is used in order to update a stream, by adding a new commit.

### Getting all commits in a stream

By calling GET on the _commits_ endpoint along with a StreamID gives you access to all of the commits of the given stream. This is useful if you want to inspect the stream history, or apply all of the commits to a Ceramic node that is not connected to the network.

=== "Request"

    ```bash
    GET /api/v0/commits/:streamid
    ```

    Here, `:streamid` should be replaced by the string representation of the StreamID of the stream that is being requested.

=== "Response"

    * `streamId` - the StreamID of the requested stream, string
    * `commits` - an array of commit objects

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/commits/kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv
    ```

=== "Response"

    ```bash
    {
      "streamId": "kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv",
      "commits": [
        {
          "cid": "bagcqcera2faj5vik2giftqxftbngfndkci7x4z5vp3psrf4flcptgkz5xztq",
          "value": {
            "jws": {
              "payload": "AXESIAsUBpZMnue1yQ0BgXsjOFyN0cHq6AgspXnI7qGB54ux",
              "signatures": [
                {
                  "signature": "16tBnfkXQU0yo-RZvfjWhm7pP-hIxJ5m-FIMHlCrRkpjbleoEcaC80Xt7qs_WZOlOCexznjow9aX4aZe51cYCQ",
                  "protected": "eyJhbGciOiJFZERTQSIsImtpZCI6ImRpZDprZXk6ejZNa2ZaNlM0TlZWVEV1dHM4bzV4RnpSTVI4ZUM2WTFibmdvQlFOblhpQ3ZoSDhII3o2TWtmWjZTNE5WVlRFdXRzOG81eEZ6Uk1SOGVDNlkxYm5nb0JRTm5YaUN2aEg4SCJ9"
                }
              ],
              "link": "bafyreialcqdjmte64624sdibqf5sgoc4rxi4d2xibawkk6oi52qydz4lwe"
            },
            "linkedBlock": "o2RkYXRhoWV0aXRsZXFNeSBmaXJzdCBEb2N1bWVudGZoZWFkZXKiZnNjaGVtYfZrY29udHJvbGxlcnOBeDhkaWQ6a2V5Ono2TWtmWjZTNE5WVlRFdXRzOG81eEZ6Uk1SOGVDNlkxYm5nb0JRTm5YaUN2aEg4SGZ1bmlxdWVwenh0b1A5blphdVgxcEE0OQ"
          }
        },
        {
          "cid": "bagcqcera3fkje7je4lvctkam4fvi675avtcuqgrv7dn6aoqljd5lebpl7rfq",
          "value": {
            "jws": {
              "payload": "AXESINm6lI30m3j5H2ausx-ulXj-L9CmFlOTZBZvJ2O734Zt",
              "signatures": [
                {
                  "signature": "zsLJbBSU5xZTQkYlXwEH9xj_t_8frvSFCYs0SlVMPXOnw8zOJOsKnJDQlUOvPJxjt8Bdc_7xoBdmcRG1J1tpCw",
                  "protected": "eyJhbGciOiJFZERTQSIsImtpZCI6ImRpZDprZXk6ejZNa2ZaNlM0TlZWVEV1dHM4bzV4RnpSTVI4ZUM2WTFibmdvQlFOblhpQ3ZoSDhII3o2TWtmWjZTNE5WVlRFdXRzOG81eEZ6Uk1SOGVDNlkxYm5nb0JRTm5YaUN2aEg4SCJ9"
                }
              ],
              "link": "bafyreigzxkki35e3pd4r6zvowmp25fly7yx5bjqwkojwiftpe5r3xx4gnu"
            },
            "linkedBlock": "pGJpZNgqWCYAAYUBEiDRQJ7VCtGQWcLlmFpitGoSP35ntX7fKJeFWJ8zKz2+Z2RkYXRhgaNib3BjYWRkZHBhdGhlL21vcmVldmFsdWUY6mRwcmV22CpYJgABhQESINFAntUK0ZBZwuWYWmK0ahI/fme1ft8ol4VYnzMrPb5nZmhlYWRlcqFrY29udHJvbGxlcnOA"
          }
        }
      ]
    }
    ```

### Applying a new commit to stream

**:octicons-alert-16: Disabled in gateway mode**

In order to modify a stream we apply a commit to its log. This commit usually contains a signature over a _json-patch_ diff describing a modification to the stream contents. The commit also needs to contain pointers to the previous commit and other metadata. You can read more about this in the [Ceramic Specification](https://github.com/ceramicnetwork/ceramic/blob/master/SPECIFICATION.md#document-records){:target="\_blank"}. Different stream types may have different formats for their commits. If you want to see an example implementation for how to construct a commit you can have a look at the implementation of the TileDocument.

=== "Request"

    ```bash
    POST /api/v0/commits
    ```

    #### Request body fields:

    - `streamId` - the StreamID of the stream to apply the commit to, string
    - `commit` - the content of the commit to apply (will differ per streamtype)
    - `opts` - options for the stream update [UpdateOpts](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.updateopts-1.html){:target="_blank"} (optional)

=== "Response"

    * `streamId` - the StreamID of the stream that was modified
    * `state` - the new state of the stream that was modified, [StreamState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.streamstate-1.html){:target="_blank"}

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/commits -X POST -d '{
      "streamId": "kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv",
      "commit": {
        "jws": {
          "payload": "AXESINm6lI30m3j5H2ausx-ulXj-L9CmFlOTZBZvJ2O734Zt",
          "signatures": [
            {
              "signature": "zsLJbBSU5xZTQkYlXwEH9xj_t_8frvSFCYs0SlVMPXOnw8zOJOsKnJDQlUOvPJxjt8Bdc_7xoBdmcRG1J1tpCw",
              "protected": "eyJhbGciOiJFZERTQSIsImtpZCI6ImRpZDprZXk6ejZNa2ZaNlM0TlZWVEV1dHM4bzV4RnpSTVI4ZUM2WTFibmdvQlFOblhpQ3ZoSDhII3o2TWtmWjZTNE5WVlRFdXRzOG81eEZ6Uk1SOGVDNlkxYm5nb0JRTm5YaUN2aEg4SCJ9"
            }
          ],
          "link": "bafyreigzxkki35e3pd4r6zvowmp25fly7yx5bjqwkojwiftpe5r3xx4gnu"
        },
        "linkedBlock": "pGJpZNgqWCYAAYUBEiDRQJ7VCtGQWcLlmFpitGoSP35ntX7fKJeFWJ8zKz2+Z2RkYXRhgaNib3BjYWRkZHBhdGhlL21vcmVldmFsdWUY6mRwcmV22CpYJgABhQESINFAntUK0ZBZwuWYWmK0ahI/fme1ft8ol4VYnzMrPb5nZmhlYWRlcqFrY29udHJvbGxlcnOA"
      }
    }' -H "Content-Type: application/json"
    ```

=== "Response"

    ```bash
    {
      "streamId": "kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv",
      "state": {
        "type": 0,
        "content": {
          "title": "My first Document"
        },
        "metadata": {
          "schema": null,
          "controllers": [
            "did:key:z6MkfZ6S4NVVTEuts8o5xFzRMR8eC6Y1bngoBQNnXiCvhH8H"
          ]
        },
        "signature": 2,
        "anchorStatus": "PENDING",
        "log": [
          {
            "cid": "bagcqcera2faj5vik2giftqxftbngfndkci7x4z5vp3psrf4flcptgkz5xztq",
            "type": 0
          },
          {
            "cid": "bagcqcera3fkje7je4lvctkam4fvi675avtcuqgrv7dn6aoqljd5lebpl7rfq",
            "type": 1
          }
        ],
        "anchorScheduledFor": "12/30/2020, 1:15:00 PM",
        "next": {
          "content": {
            "title": "My first Document",
            "more": 234
          },
          "metadata": {
            "schema": null,
            "controllers": []
          }
        }
      }
    }
    ```

## **Pins API**

The `pins` api endpoint can be used to manipulate the pinset. The pinset is all of the streams that a node maintains the state of. Any stream opened by the node that is not pinned will eventually be garbage collected from the node.

### Adding to pinset

**:octicons-alert-16: Disabled in gateway mode**

This method adds the stream with the given StreamID to the pinset.

=== "Request"

    ```bash
    POST /api/v0/pins/:streamid
    ```

    Here, `:streamid` should be replaced by the string representation of the StreamID of the stream that is being requested.

=== "Response"

    If the operation was sucessful the response will be a 200 OK.

    * `streamId` - the StreamID of the stream which was pinned, string

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/pins/k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl -X POST
    ```

=== "Response"

    ```bash
    {
      "streamId": "k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl"
    }
    ```

### Removing from pinset

**:octicons-alert-16: Disabled in gateway mode**

This method removes the stream with the given StreamID from the pinset.

=== "Request"

    ```bash
    DELETE /api/v0/pins/:streamid
    ```

    Here, `:streamid` should be replaced by the string representation of the StreamID of the stream that is being requested.

=== "Response"

    If the operation was sucessful the response will be a 200 OK.

    * `streamId` - the StreamID of the stream which was unpinned, string

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/pins/k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl -X DELETE
    ```

=== "Response"

    ```bash
    {
      "streamId": "k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl"
    }
    ```

### Listing streams in pinset

Calling this method allows you to list all of the streams that are in the pinset on this node.

=== "Request"

    ```bash
    GET /api/v0/pins
    ```

=== "Response"

    * `pinnedStreamIds` - an array of StreamID strings that are in the pinset

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/pins
    ```

=== "Response"

    ```bash
    {
      "pinnedStreamIds": [
        "k2t6wyfsu4pfwqaju0w9nmi53zo6f5bcier7vc951x4b9rydv6t8q4pvzd5w3l",
        "k2t6wyfsu4pfxon8reod8xcyka9bujeg7acpz8hgh0jsyc7p2b334izdyzsdp7",
        "k2t6wyfsu4pfxqseec01fnqywmn8l93p4g2chzyx3sod3hpyovurye9hskcegs",
        "k2t6wyfsu4pfya9y0ega1vnokf0g5qaus69basy52oxg50y3l35vm9rqbb88t3"
      ]
    }
    ```

### Checking inclusion in pinset

This method is used to check if a particular stream is in the pinset.

=== "Request"

    ```bash
    GET /api/v0/pins/:streamid
    ```

    Here, `:streamid` should be replaced by the string representation of the StreamID of the stream that is being requested.

=== "Response"

    * `pinnedStreamIds` - an array containing the specified StreamID string if that stream is pinned, or an empty array if that stream is not pinned

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/pins/k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl
    ```

=== "Response"

    ```bash
    {
      "pinnedStreamIds": ["k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl"]
    }
    ```

## **Node Info APIs**

The methods under the `/node` path provides more information about this particular node.

### Supported blockchains for anchoring

Get all of the [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) _chainIds_ supported by this node.

=== "Request"

    ```bash
    GET /api/v0/node/chains
    ```

=== "Response"

    The response body contains the following fields:

    - `supportedChains` - and array with [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) formatted chainIds

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/node/chains
    ```

=== "Response"

    ```bash
    {
      "supportedChains": ["eip155:3"]
    }
    ```

### Health check

Check the health of the node and the machine it's running on. Run `ceramic daemon -h` for more details on how this can be configured.

=== "Request"

    ```bash
    GET /api/v0/node/healthcheck
    ```

=== "Response"

    Either a `200` response with the text `Alive!`, or a `503` with the text `Insufficient resources`.

#### Example

=== "Request"

    ```bash
    curl http://localhost:7007/api/v0/node/healthcheck
    ```

=== "Response"

    ```bash
    Alive!
    ```
