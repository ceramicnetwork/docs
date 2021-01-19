# HTTP API

The HTTP API allows you to manually make HTTP requests that create, modify, and query documents on a remote Ceramic node. If you are building an application, you will usually interact with a node using one of the [Ceramic clients](../javascript/clients.md), however this documentation is useful if:

- You have a special use case where you directly want to use HTTP requests
- You want to implement an HTTP client in a new language

!!! info "Gateway mode"
    Not all of the API methods are available if the Ceramic node runs in
    *gateway mode*. This option disables writes, which is useful when exposing your node to
    the internet. **Methods disabled in gateway mode will be clearly marked.**

## **Documents**

The `documents` endpoint is used to create new documents, load documents from their DocID or from their genesis content. 

### Get document state
Load the state of a document given its DocID.

=== "Request"
    ```
    GET /api/v0/documents/:docid
    ```

    Here, `:docid` should be replaced by the string representation of the DocID of the document that is being requested.

=== "Response"
    The response body contains the following fields:

    - `docId` - the DocID of the requested document as string
    - `state` - the state of the requested document as [DocState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docstate-1.html){:target="_blank"}

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/documents/kjzl6cwe1jw147r7878h32yazawcll6bxe5v92348cxitif6cota91qp68grbhm
    ```

=== "Response"
    ```bash
    {
      "docId": "kjzl6cwe1jw147r7878h32yazawcll6bxe5v92348cxitif6cota91qp68grbhm",
      "state": {
        "doctype": "tile",
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

### Create document
Create a new document, or load a document from its genesis content. The genesis content may be signed (a DagJWS for the `tile` doctype), or unsigned in some cases.

=== "Request"
    ```bash
    POST /api/v0/documents
    ```

    #### Request body fields:

    - `doctype` - the name of the doctype to use (e.g. 'tile'), string
    - `genesis` - the genesis content of the document (will differ per doctype)
    - `docOpts` - options for the document creation, [DocOpts](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docopts-1.html){:target="_blank"} (optional)

=== "Response"

    The response body contains the following fields:

    - `docId` - the DocID of the requested document as string
    - `state` - the state of the requested document as [DocState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docstate-1.html){:target="_blank"}

#### Example

This example creates a `tile` document from an unsigned genesis commit. Note that if the content is defined for a `tile` genesis commit, it needs to be signed.

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/documents -X POST -d '{
        "doctype": "tile",
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
      "docId": "k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl",
      "state": {
        "doctype": "tile",
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

## **Multiqueries**
The `multiqueries` endpoint enables querying multiple documents at once, as well as querying documents which are linked.

### Query multiple documents
This endpoint allows you to query multiple DocIDs. Along with each DocID an array of paths can be passed. If any of the paths within the document structure contains a ceramic DocID url (`ceramic://<DocID>`), this linked document will also be returned as part of the response.

=== "Request"
    ```bash
    POST /api/v0/multiqueries
    ```

    #### Request body fields:
    - `queries` - an array of [MultiQuery](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.multiquery-1.html){:target="_blank"} objects

=== "Response"

    The response body contains a map from DocID strings to [DocState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docstate-1.html){:target="_blank"} objects.

#### Example

First let's create three documents to query using the ceramic cli:

=== "Request1"
    ```bash
    $ ceramic create tile --content '{ "Document": "A" }'
    ```

=== "Response1"
    ```bash
    DocID(kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc)
    {
      "Document": "A"
    }
    ```

=== "Request2"
    ```bash
    $  ceramic create tile --content '{ "Document": "B" }'
    ```

=== "Response2"
    ```bash
    DocID(kjzl6cwe1jw147w3xz3xrcd37chh2rz4dfra3imtnsni385rfyqa3hbx42qwal0)
    {
      "Document": "B"
    }

    ```

=== "Request3"
    ```bash
    $ ceramic create tile --content '{
        "Document": "C",
        "link": "ceramic://kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc"
      }'
    ```

=== "Response3"
    ```bash
    DocID(kjzl6cwe1jw14b54pb10voc4bqh73qyu8o6cfu66hoi3feidbbj81i5rohh7kgl)
    {
      "link": "ceramic://kjzl6cwe1jw149rledowj0zi0icd7epi9y1m5tx4pardt1w6dzcxvr6bohi8ejc",
      "Document": "C"
    }
    ```

Now let's query them though the multiqueries endpoint:

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/multiqueries -X POST -d '{
      "queries": [{
        "docId": "kjzl6cwe1jw14b54pb10voc4bqh73qyu8o6cfu66hoi3feidbbj81i5rohh7kgl",
        "paths": ["link"]
      }, {
        "docId": "kjzl6cwe1jw147w3xz3xrcd37chh2rz4dfra3imtnsni385rfyqa3hbx42qwal0",
        "paths": []
      }]
    }' -H "Content-Type: application/json"
    ```

=== "Response"
    ```bash
    {
      "kjzl6cwe1jw14b54pb10voc4bqh73qyu8o6cfu66hoi3feidbbj81i5rohh7kgl": {
        "doctype": "tile",
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
        "doctype": "tile",
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
        "doctype": "tile",
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



## **Commits**

The `commits` endpoint provides lower level access to the data structure of a Ceramic document. It is also the enpoint that is used in order to update a document, by adding a new commit.

### Get all document commits

By calling get on the *commits* endpoint along with a DocID gives you access to all of the commits of the given document. This is useful if you want to inspect the document history, or apply all of the commits to a ceramic node that is not connected to the network.

=== "Request"
    ```bash
    GET /api/v0/commits/:docid
    ```

    Here, `:docid` should be replaced by the string representation of the DocID of the document that is being requested.

=== "Response"

    * `docId` - the DocID of the requested document, string
    * `commits` - an array of commit objects

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/commits/kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv
    ```

=== "Response"
    ```bash
    {
      "docId": "kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv",
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

### Apply a commit to document

**:octicons-alert-16: Disabled in gateway mode**

In order to modify a document we apply a commit to its docment log. This commit usually contains a signature over a *json-patch* diff describing a modification to the document contents. The commit also needs to contain pointers to the previous commit and other metadata. You can read more about this in the [Ceramic Specification](https://github.com/ceramicnetwork/ceramic/blob/master/SPECIFICATION.md#document-records){:target="_blank"}. Different document types may have different formats for their commits. If you want to see an example implementation for how to construct a commit you can have a look at the implementation of the TileDoctype.

=== "Request"
    ```bash
    POST /api/v0/commits
    ```

    #### Request body fields:

    - `docId` - the name of the doctype to use, string
    - `commit` - the content of the commit to apply (will differ per doctype)
    - `docOpts` - options for the document update [DocOpts](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docopts-1.html){:target="_blank"} (optional)

=== "Response"

    * `docId` - the DocID of the document that was modified
    * `state` - the new state of the document that was modified, [DocState](https://developers.ceramic.network/reference/typescript/interfaces/_ceramicnetwork_common.docstate-1.html){:target="_blank"}

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/commits -X POST -d '{
      "docId": "kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv",
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
      "docId": "kjzl6cwe1jw14ahmwunhk9yjwawac12tb52j1uj3b9a57eohmhycec8778p3syv",
      "state": {
        "doctype": "tile",
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



## **Pins**

The `pins` api endpoint can be used to manipulate the pinset. The pinset is all of the documents that a node maintains the state of. Any document opened by the node that is not pinned will eventually be garbage collected from the node.

### Add to pinset
This method adds the document with the given DocID to the pinset.

**:octicons-alert-16: Disabled in gateway mode**

=== "Request"
    ```bash
    POST /api/v0/pins/:docid
    ```

    Here, `:docid` should be replaced by the string representation of the DocID of the document that is being requested.

=== "Response"

    If the operation was sucessful the response will be a 200 OK.

    * `docId` - the DocID of the document which was pinned, string

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/pins/k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl -X POST
    ```

=== "Response"
    ```bash
    {
      "docId": "k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl"
    }
    ```

### Remove from pinset
This method removes the document with the given DocID from the pinset.

**:octicons-alert-16: Disabled in gateway mode**

=== "Request"
    ```bash
    DELETE /api/v0/pins/:docid
    ```

    Here, `:docid` should be replaced by the string representation of the DocID of the document that is being requested.

=== "Response"

    If the operation was sucessful the response will be a 200 OK.

    * `docId` - the DocID of the document which was unpinned, string

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/pins/k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl -X DELETE
    ```

=== "Response"
    ```bash
    {
      "docId": "k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl"
    }
    ```

### List documents in pinset

Calling this method allows you to list all of the documents that are in the pinset on this node.

=== "Request"

    ```bash
    GET /api/v0/pins
    ```

=== "Response"

    * `pinnedDocIds` - an array of DocID strings that are in the pinset

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/pins
    ```

=== "Response"
    ```bash
    {
      "pinnedDocIds": [
        "k2t6wyfsu4pfwqaju0w9nmi53zo6f5bcier7vc951x4b9rydv6t8q4pvzd5w3l",
        "k2t6wyfsu4pfxon8reod8xcyka9bujeg7acpz8hgh0jsyc7p2b334izdyzsdp7",
        "k2t6wyfsu4pfxqseec01fnqywmn8l93p4g2chzyx3sod3hpyovurye9hskcegs",
        "k2t6wyfsu4pfya9y0ega1vnokf0g5qaus69basy52oxg50y3l35vm9rqbb88t3"
      ]
    }
    ```

### Confirm document in pinset

This method is used to check if a particular document is in the pinset.

=== "Request"

    ```bash
    GET /api/v0/pins/:docid
    ```

    Here, `:docid` should be replaced by the string representation of the DocID of the document that is being requested.

=== "Response"

    * `pinnedDocIds` - an array containing the specified DocID string if that document is pinned, or an empty array if that document is not pinned

#### Example

=== "Request"
    ```bash
    $ curl http://localhost:7007/api/v0/pins/k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl
    ```

=== "Response"
    ```bash
    {
      "pinnedDocIds": ["k2t6wyfsu4pg2qvoorchoj23e8hf3eiis4w7bucllxkmlk91sjgluuag5syphl"]
    }
    ```

## **Node Info**

The methods under the `/node` path provides more information about this particular node.

### Supported blockchains for anchoring
Get all of the [CAIP-2](https://github.com/ChainAgnostic/CAIPs/blob/master/CAIPs/caip-2.md) *chainIds* supported by this node.

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
    $ curl http://localhost:7007/api/v0/node/chains
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
    $ curl http://localhost:7007/api/v0/node/healthcheck
    ```

=== "Response"
    ```bash
    Alive!
    ```

</br>
</br>
</br>
