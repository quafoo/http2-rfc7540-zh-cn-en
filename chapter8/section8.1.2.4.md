# Response Pseudo-Header Fields
> For HTTP/2 responses, a single :status pseudo-header field is defined that carries the HTTP status code field (see [RFC7231], Section 6). This pseudo-header field MUST be included in all responses; otherwise, the response is malformed (Section 8.1.2.6).

> HTTP/2 does not define a way to carry the version or reason phrase that is included in an HTTP/1.1 status line.