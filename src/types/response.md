# Response
- the response message is sent by the server to the client
- it is sent in response to a `request` message
- the response message is in the form of a JSON object
## Response Format
```json
{
    "jsontp": "1.0",
    "type": "response",
    "status": {
        "code": 200,
        "formal-message": "OK",
        "human-message": "The request was successful."
    },
    "resource": "/path/to/resource",
    "headers": {
        "date": "2024-01-01T00:00:00Z+00:00",
        "language": "en-GB",
    },
    "body": {
        "key1": "value1",

        "content": "raw text to be sent",

        "encoding": "gzip"
    }
}
```
## Fields
- if a required field is not present, the client might not be able to process the response, so it is vital that all fields are present
### `jsontp` - `string`, required
- the version of the `jsontp` standard that the message follows
- it is always in the form `major.minor(-rcx)`, where `x` is the release candidate number
- for example, `1.0`, `1.1`, `1.2-rc1`, etc.
### `type` - `string`, required
- the type of message, which in this case is `response`.
### `status` - `object`, required
- the status of the response
- it has three required subfields - `code`, `formal-message`, and `human-message`
#### `code` - `number`, required
- the status code of the response, which must be a valid HTTP status code
- below are some examples of valid `code` fields:
    - `200`
    - `404`
    - `500`
    - `418`
    - `503`
#### `formal-message` - `string`, required
- the formal message of the status code
- this is the message corresponding to the status code
- below are some examples of valid `formal-message` fields:
    - `OK`
    - `Not Found`
    - `Internal Server Error`
    - `I'm a teapot`
    - `Service Unavailable`
- this is case-insensitive
#### `human-message` - `string`, required
- the human-readable message of the status code
- this is a message that is intended for the user to read, and is not intended to be parsed by a machine
- therefore, it is not standardised, and can be anything, at the discretion of the server
- below are some examples of valid `human-message` fields:
    - `The request was successful.`
    - `The resource was not found.`
    - `The server encountered an error.`
    - `I'm a teapot.`
    - `The server is currently unavailable.`
### `resource` - `string`, required
- the path to the resource that the client is requesting
- though this was sent in the `request` message, it is also sent in the `response` message, so that the client can verify that the server is responding to the correct request, and the data has not been mangled in transit, or mistaken by the server
### `headers` - `object`, required
- the headers to send with the response
- the keys are the header names, and the values are the header values
- in the `response` field there are two required headers - `date` and `language`
#### `date` - `string`, required
- the date and time that the response was sent
- it is in the form `YYYY-MM-DDTHH:MM:SSZ+0000`, where `Z` is the timezone offset
- it should be in the `UTC` timezone, but if it is not, the server should include the timezone offset, with no colon and no space between the `Z` and the offset
- for example, `2024-01-01T00:00:00Z+0000`, `2024-01-01T00:00:00Z-0500`, `2024-01-01T00:00:00Z+0530`, etc.
#### `language` - `string`, required
- the `language` header is used to tell the client the language of the response.
- the language must be in the format `ISO 639-1` language code, followed by a hyphen, and then a valid `ISO 3166 Alpha 2` country code.
- for example, `en-GB`, `en-US`, `fr-FR`, `de-DE`, etc.
