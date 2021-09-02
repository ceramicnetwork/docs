# Basic Profile

The basic profile schema is used to create a stream that stores basic profile metadata for a DID. It is typically deployed in a Tile Document StreamType, and is one of the default schemas provided in the IDX identity protocol.

See [CIP-19](https://github.com/ceramicnetwork/CIP/blob/main/CIPs/CIP-19/CIP-19.md) for the complete specification.

## **Schema**

The basic profile schema defines the format of a stream that contains the properties listed below. Properties not defined in the schema _cannot_ be included in the basic profile, however the schema can always by modifying the CIP.

| Property           | Description                    | Value                                                                                  | Max Size | Required | Example                      |
| ------------------ | ------------------------------ | -------------------------------------------------------------------------------------- | -------- | -------- | ---------------------------- |
| `name`             | a name                         | string                                                                                 | 150 char | optional | Mary Smith                   |
| `image`            | an image                       | Image sources                                                                          |          | optional |                              |
| `description`      | a short description            | string                                                                                 | 420 char | optional | This is my cool description. |
| `emoji`            | an emoji                       | unicode                                                                                | 2 char   | optional | 🔢                           |
| `background`       | a background image (3:1 ratio) | Image sources                                                                          |          | optional |                              |
| `birthDate`        | a date of birth                | [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)                                     | 10 char  | optional | 1990-04-24                   |
| `url`              | a url                          | string                                                                                 | 240 char | optional | http://ceramic.network       |
| `gender`           | a gender                       | string                                                                                 | 42 char  | optional | female                       |
| `residenceCity`    | a city of residence            | string                                                                                 | 140 char | optional | Berlin                       |
| `residenceCountry` | a country of residence         | [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2)                 | 2 char   | optional | DE                           |
| `nationalities`    | nationalities                  | array of [ISO 3166-1 alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) values | 2 char   | optional | CN                           |
| `affiliations`     | affiliations                   | array of strings                                                                       | 240 char | optional | Ceramic Ecosystem Alliance   |

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "BasicProfile",
  "type": "object",
  "definitions": {
    "IPFSUrl": {
      "type": "string",
      "pattern": "^ipfs://.+",
      "maxLength": 150
    },
    "positiveInteger": {
      "type": "integer",
      "minimum": 1
    },
    "imageMetadata": {
      "type": "object",
      "properties": {
        "src": {
          "$ref": "#/definitions/IPFSUrl"
        },
        "mimeType": {
          "type": "string",
          "maxLength": 50
        },
        "width": {
          "$ref": "#/definitions/positiveInteger"
        },
        "height": {
          "$ref": "#/definitions/positiveInteger"
        },
        "size": {
          "$ref": "#/definitions/positiveInteger"
        }
      },
      "required": ["src", "mimeType", "width", "height"]
    },
    "imageSources": {
      "type": "object",
      "properties": {
        "original": {
          "$ref": "#/definitions/imageMetadata"
        },
        "alternatives": {
          "type": "array",
          "items": {
            "$ref": "#/definitions/imageMetadata"
          }
        }
      },
      "required": ["original"]
    }
  },
  "properties": {
    "name": {
      "type": "string",
      "maxLength": 150
    },
    "image": {
      "$ref": "#/definitions/imageSources"
    },
    "description": {
      "type": "string",
      "maxLength": 420
    },
    "emoji": {
      "type": "string",
      "maxLength": 2
    },
    "background": {
      "$ref": "#/definitions/imageSources"
    },
    "birthDate": {
      "type": "string",
      "format": "date"
    },
    "url": {
      "type": "string",
      "maxLength": 240
    },
    "gender": {
      "type": "string",
      "maxLength": 42
    },
    "homeLocation": {
      "type": "string",
      "maxLength": 140
    },
    "residenceCountry": {
      "type": "string",
      "pattern": "^[A-Z]{2}$"
    },
    "nationalities": {
      "type": "array",
      "minItems": 1,
      "items": {
        "type": "string",
        "pattern": "^[A-Z]{2}$",
        "maxItems": 5
      }
    },
    "affiliations": {
      "type": "array",
      "items": {
        "type": "string",
        "maxLength": 240
      }
    }
  }
}
```

## **Usage**

There are two primary ways to create basic profile streams.

### With IDX (recommended)

IDX is a protocol and SDK that provides a way for developers to build user-centric applications by organizing information around a DID in an application-agnostic way, allowing them to set and get identity records. At its core, IDX functions like a user-centric, decentralized user table shared by all applications which enables maximal interoperability of information. Basic profile is a default schema provided by IDX. See the [**IDX documentation**](https://developers.idx.xyz) for how to install IDX in your project.

### Without IDX

You can create streams that conform to the basic profile schema without using IDX, if desired. However, note that without using IDX it will be difficult for your application or other applications to keep track of the StreamIDs of the basic profiles for each of your users. You will need to do this manually.

To create a basic profile without using IDX, manually create a Tile Document stream using a Ceramic client and include the StreamID of the basic profile schema in the stream's metadata:

`k3y52l7qbv1frxt706gqfzmq6cbqdkptzk8uudaryhlkf6ly9vx21hqu4r6k1jqio`

#### Example

An example of how to create a basic profile stream with js-ceramic.

```js
const profile = await ceramic.createDocument('tile', {
  metadata: {
    schema: "<record-schema-DocID>"
    family: "<definition-DocID>"
  },
  content: {
    name: "Samantha Smith",
    image: {
      original: {
        src: "ipfs://bafy...",
        mimeType: "image/png",
        width: 500,
        height: 200
      }
    },
    description: "This is my funny description.",
    emoji: "🚀",
    url: "http://ceramic.network"
  }
})
```
