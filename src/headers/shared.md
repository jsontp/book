# Shared headers
## `content-type`
- The `content-type` header is used to specify the type of the content in the message body.
- It is a valid MIME type.
- If it is sent from the client, and the server does not support the specified type, it should respond with a `415 Unsupported Media Type` status code.