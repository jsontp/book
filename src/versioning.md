# jsontp versioning
- the jsontp version is in the format major.minor(-rcx)
- the major version is incremented when there are breaking changes
the minor version is incremented when there are non-breaking changes (though this may never happen)
- the rcx is the release candidate number, and will only be present in a release candidate version, for example, 1.0-rc1. It will not be present in a stable release. If it is present, it should be incremented with each release candidate, and omitted in the stable release.
- the jsontp version is always in the jsontp key-value pair, which is mandatory in all jsontp messages.
- if the jsontp version is not supported, the server should respond with a 505 HTTP Version Not Supported status code.
- if the jsontp version is not provided, the server should respond with a 400 Bad Request status code.
- examples: `1.0`, `1.2-rc3`, `2.3`