# Status Codes
- jsontp uses HTTP status codes to indicate the success or failure of a request.
- any valid HTTP status code is a valid jsontp status code - the only difference is that the message body is in JSON format.
- while any status code can be used as the server sees fit, there are a few mentioned in the specification that are used in a specific way.
## `505 HTTP Version Not Supported`
- if the server does not support the version of jsontp that the client is using, it should respond with a `505 HTTP Version Not Supported` status code.
## `400 Bad Request`
- if the server does not understand the request, it should respond with a `400 Bad Request` status code.
- this is mentioned numerous times in the specification, and is used in a variety of contexts.
## `405 Method Not Allowed`
- if the server does not support the method that the client is using, it should respond with a `405 Method Not Allowed` status code.
## `406 Not Acceptable`
- if the server does not support the language that the client is using, it should respond with a `406 Not Acceptable` status code.
## `412 Precondition Failed`
- used for conditional requests that cannot be met, or also in the case of the `if-unmodified-since` header when the resource has been modified
## `415 Unsupported Media Type`
- used for unsupported MIME types
## `404 Not Found`
- used for resources that do not exist
## `304 Not Modified`
- used for conditional requests that have not been modified
## `501 Not Implemented`
- used for unsupported conditions, such as the `expect` header
