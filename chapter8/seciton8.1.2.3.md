# Request Pseudo-Header Fields
> The following pseudo-header fields are defined for HTTP/2 requests:
> 
> * The :method pseudo-header field includes the HTTP method ([RFC7231], Section 4).
> 
> * The :scheme pseudo-header field includes the scheme portion of the target URI ([RFC3986], Section 3.1).
> 
> * :scheme is not restricted to http and https schemed URIs. A proxy or gateway can translate requests for non-HTTP schemes, enabling the use of HTTP to interact with non-HTTP services.
> 
> * The :authority pseudo-header field includes the authority portion of the target URI ([RFC3986], Section 3.2). The authority MUST NOT include the deprecated userinfo subcomponent for http or https schemed URIs.
> 
> 	To ensure that the HTTP/1.1 request line can be reproduced accurately, this pseudo-header field MUST be omitted when translating from an HTTP/1.1 request that has a request target in origin or asterisk form (see [RFC7230], Section 5.3). Clients that generate HTTP/2 requests directly SHOULD use the :authority pseudo-header field instead of the Host header field. An intermediary that converts an HTTP/2 request to HTTP/1.1 MUST create a Host header field if one is not present in a request by copying the value of the :authority pseudo-header field.
> 
> * The :path pseudo-header field includes the path and query parts of the target URI (the path-absolute production and optionally a '?' character followed by the query production (see Sections 3.3 and 3.4 of [RFC3986]). A request in asterisk form includes the value '*' for the :path pseudo-header field.
> 
> 	This pseudo-header field MUST NOT be empty for http or https URIs; http or https URIs that do not contain a path component MUST include a value of '/'. The exception to this rule is an OPTIONS request for an http or https URI that does not include a path component; these MUST include a :path pseudo-header field with a value of '*' (see [RFC7230], Section 5.3.4).

> All HTTP/2 requests MUST include exactly one valid value for the :method, :scheme, and :path pseudo-header fields, unless it is a CONNECT request (Section 8.3). An HTTP request that omits mandatory pseudo-header fields is malformed (Section 8.1.2.6).

> HTTP/2 does not define a way to carry the version identifier that is included in the HTTP/1.1 request line.

