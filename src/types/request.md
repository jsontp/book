# Request
- the request message is sent from the client to the server
- it is a `JSON` object, and follows a specific format, which is discussed line by line below
## Request Format
```json
{
    "jsontp": "1.0",
    "type": "request",
    "resource": "/path/to/resource",
    "method": "GET",

    "headers": { 
        "key1": "value1",
    },

    "body": {
        "key1": "value1", 

        "content": "raw text to be sent",

        "encoding": "gzip"
    }
}
```
## Fields
- if a required field is not present, the server will respond with a `400 Bad Request` status code
- if a field is provided that is not in the list below, the server will respond with a `400 Bad Request` status code
- if a field is in an incorrect format, or is an incorrect type, the server will respond with a `400 Bad Request` status code
### `jsontp` - `string`, required
- the version of the `jsontp` standard that the message follows
- it is always in the form `major.minor(-rcx)`, where `x` is the release candidate number
- for example, `1.0`, `1.1`, `1.2-rc1`, etc.
### `type` - `string`, required
- the type of message, which in this case is `request`.
### `resource` - `string`, required
- the path to the resource that the client is requesting
- below are some examples of valid `resource` fields:
    - `/`
    - `/index.html`
    - `example.com/index.html`
    - `example.com:8080/index.html`
    - `example.com:8080/route?query=string`
    - `example.com:8080/route?query=string#fragment`
    - `path/to/resource`
    - `jsontp://example.com:8080/path/to/resource`
- different servers may handle this differently, so it is ideal to send requests in the first or second format, which is `/` followed if necessary by the path to the resource
### `method` - `string`, required
- the method to use when making the request
- below are the list of HTTP methods mentioned in the standard:
    - `GET`
    - `POST`
    - `PUT`
    - `DELETE`
    - `OPTIONS`
- bear in mind that the server may not support all of these methods, as it only is required to support `GET` and `POST`. If the server does not support the method, it will respond with a `405 Method Not Allowed` status code
### `headers` - `object`, required
- the headers to send with the request
- the keys are the header names, and the values are the header values
- in the `request` there are no required headers
- below are some examples of valid `headers` fields:
    - `{}`
    - `{"key1": "value1"}`
    - `{"key1": "value1", "key2": "value2"}`
- all header keys are case-insensitive, so the server will treat `Content-Type` and `content-type` as the same header
- all header values are strings, so if a header value is not a string, the server will respond with a `400 Bad Request` status code
- if a header is not in the list of headers that the standard mentions, the server will respond with a `400 Bad Request` status code
- if a header is merely not supported by the server, the server will ignore the header, or respond with a `501 Not Implemented` status code
### `body` - `object`, required
- the body of the request
- the body can contain key-value pairs following the same rules as the `headers` field
- however, the body must also contain two subfields, `content` and `encoding`
#### `content` - `string`, required
- the raw text to be sent as the body of the request.
- if the `content` field is not present, the request is either a `POST` or `PUT` request, and the header `"expect": "100-continue"` is not present, the server will respond with a `400 Bad Request` status code
#### `encoding` - `string`, required
- the encoding of the `content` field
- below are the list of allowed encodings:
    - `gzip`
    - `deflate`
    - `br` (Google's Brotli)
    - `identity` (no encoding)
- if the encoding is not in the list above, the server will respond with a `412 Precondition Failed` status code
## The 100-Continue Header
- if the client is planning to send a large request, it can send the header `"expect": "100-continue"` to the server
- this tells the server to bear in mind that the client is planning to send a large request, and so it should not timeout as quickly
- if the server does not support the `100-Continue` header, it will respond with a `501 Not Implemented` status code
- if the server supports the `100-Continue` header, it will respond with a `100 Continue` status code, and the client can then send the full request, in the same format as above. Notably different from HTTP, the _full_ request is sent, not just the body
- example of a request with the `100-Continue` header:
```json
{
    "jsontp": "1.0",
    "type": "request",
    "resource": "/path/to/resource",
    "method": "GET",

    "headers": {
        "expect": "100-continue" // required (obviously!)
    },

    "body": {} // if not empty, it will be ignored
}
```
- once the server responds with a `100 Continue` status code, the client can send the full request, in the same format as above
## Example Request
```json
{
    "jsontp": "1.0",
    "type": "request",
    "resource": "/index.html",
    "method": "POST",

    "headers": {
        "accept": "text/html",
        "accept-encoding": "gzip, deflate",
    },

    "body": {
        "content": "key1=value1&key2=value2&key3=value3",
        "encoding": "identity"
    }
}
```