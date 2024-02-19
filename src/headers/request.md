# Request headers
- all request headers are optional
## `accept-encoding` - `array` or `string`
- the `accept-encoding` header is used to specify the encoding that the client can accept
- it is a list of encodings, in order of preference
- if it is a string, it is a single encoding
- if it is an array, it is a list of encodings
- if the server does not support the specified encoding, it should respond with a `412 Precondition Failed` status code
- valid encodings are `gzip`, `deflate`, `br`, `identity` (no encoding)
## `accept-language` - `array` or `string`
- the `accept-language` header is used to specify the language that the client can accept
- much like the `accept-encoding` header, it is a list of languages, in order of preference
- if the server does not support the specified language, it should respond with a `406 Not Acceptable` status code
- valid languages are ISO 639-1 language codes, followed by a hyphen, and then a valid ISO 3166 Alpha 2 country code
- e.g. `en-US`, `fr-CA`
## `authorization` - `string`
- the `authorization` header is used to specify the credentials that the client is using to authenticate itself
- the value is a string, the exact format of which is determined by the server's configuration
## `cookies` - `object`
- the `cookies` header is used to specify the cookies that the client has
- it is an object, where the keys are the names of the cookies, and the values are the cookies themselves
- e.g. `{"cookie1": "value1", "cookie2": "value2"}`
- if they are in an invalid format, the server will respond with a `400 Bad Request` status code
## `if-modified-since` - `string`
- the `if-modified-since` header is used to tell the server to only send the message if it has been modified since the specified date
- the value is a string, in the format `%Y-%m-%dT%H:%M:%SZ%z`
- if it has not been modified, the server will respond with a `304 Not Modified` status code
## `if-unmodified-since` - `string`
- the `if-unmodified-since` header is used to tell the server to only send the message if it has not been modified since the specified date
- the value is a string, in the format `%Y-%m-%dT%H:%M:%SZ%z`
- if it has been modified, the server will respond with a `412 Precondition Failed` status code
## `expect` - `string` (only `100-continue` is supported)
- the `expect` header is used to tell the server to only send the message if the specified condition is met
- the only supported value is `100-continue`
- if the server does not support the specified condition, it should respond with a `501 Not Implemented` status code
## `ignore-invalid-headers` - `boolean`
- the `ignore-invalid-headers` header is used to tell the server to ignore any headers that it does not understand
- if set to `true`, the server will ignore any headers that it does not understand
- if set to `false`, the server will respond with a `400 Bad Request` status code if it encounters any headers that it does not understand
- should be `false` by default