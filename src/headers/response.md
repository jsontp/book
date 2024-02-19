# Response headers
## `date` - `string`, required
- the `date` header is used to specify the date and time at which the message was sent.
- it is always in the format `%Y-%m-%dT%H:%M:%SZ%z`.
- it should be in UTC time.
- e.g. `2019-01-01T00:00:00Z+0000`
## `language` - `string`, required
- the `language` header is used to specify the language of the message body.
- it is in the format ISO 639-1 language code, followed by a hyphen, and then a valid ISO 3166 Alpha 2 country code.
- e.g. `en-US`, `fr-CA`
## `set-cookies` - `object`, required
- the `set-cookies` header is used to specify the cookies that should be set on the client.
- it is an object, where the keys are the names of the cookies, and the values are the cookies themselves.
- e.g. `{"cookie1": "value1", "cookie2": "value2"}`
- if they are in an invalid format, the server will respond with a `400 Bad Request` status code.