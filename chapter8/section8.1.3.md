# Examples
> This section shows HTTP/1.1 requests and responses, with illustrations of equivalent HTTP/2 requests and responses.

> An HTTP GET request includes request header fields and no payload body and is therefore transmitted as a single HEADERS frame, followed by zero or more CONTINUATION frames containing the serialized block of request header fields. The HEADERS frame in the following has both the END_HEADERS and END\_STREAM flags set; no CONTINUATION frames are sent.

> ```
>  GET /resource HTTP/1.1           HEADERS
>  Host: example.org          ==>     + END_STREAM
>  Accept: image/jpeg                 + END_HEADERS
>                                       :method = GET
>                                       :scheme = https
>                                       :path = /resource
>                                       host = example.org
>                                       accept = image/jpeg
> ```

> Similarly, a response that includes only response header fields is transmitted as a HEADERS frame (again, followed by zero or more CONTINUATION frames) containing the serialized block of response header fields.

> ```
>  HTTP/1.1 304 Not Modified        HEADERS
>  ETag: "xyzzy"              ==>     + END_STREAM
>  Expires: Thu, 23 Jan ...           + END_HEADERS
>                                       :status = 304
>                                       etag = "xyzzy"
>                                       expires = Thu, 23 Jan ...
> ```

> An HTTP POST request that includes request header fields and payload data is transmitted as one HEADERS frame, followed by zero or more CONTINUATION frames containing the request header fields, followed by one or more DATA frames, with the last CONTINUATION (or HEADERS) frame having the END_HEADERS flag set and the final DATA frame having the END_STREAM flag set:

> ```
>  POST /resource HTTP/1.1          HEADERS
>  Host: example.org          ==>     - END_STREAM
>  Content-Type: image/jpeg           - END_HEADERS
>  Content-Length: 123                  :method = POST
>                                       :path = /resource
>  {binary data}                        :scheme = https
> 
>                                   CONTINUATION
>                                     + END_HEADERS
>                                       content-type = image/jpeg
>                                       host = example.org
>                                       content-length = 123
> 
>                                   DATA
>                                     + END_STREAM
>                                   {binary data}
> ```

> Note that data contributing to any given header field could be spread between header block fragments. The allocation of header fields to frames in this example is illustrative only.

> A response that includes header fields and payload data is transmitted as a HEADERS frame, followed by zero or more CONTINUATION frames, followed by one or more DATA frames, with the last DATA frame in the sequence having the END_STREAM flag set:

> ```
>  HTTP/1.1 200 OK                  HEADERS
>  Content-Type: image/jpeg   ==>     - END_STREAM
>  Content-Length: 123                + END_HEADERS
>                                       :status = 200
>  {binary data}                        content-type = image/jpeg
>                                       content-length = 123
> 
>                                   DATA
>                                     + END_STREAM
>                                   {binary data}
> ```

> An informational response using a 1xx status code other than 101 is transmitted as a HEADERS frame, followed by zero or more CONTINUATION frames.

> Trailing header fields are sent as a header block after both the request or response header block and all the DATA frames have been sent. The HEADERS frame starting the trailers header block has the END_STREAM flag set.

> The following example includes both a 100 (Continue) status code, which is sent in response to a request containing a "100-continue" token in the Expect header field, and trailing header fields:

> ```
>  HTTP/1.1 100 Continue            HEADERS
>  Extension-Field: bar       ==>     - END_STREAM
>                                     + END_HEADERS
>                                       :status = 100
>                                       extension-field = bar
> 
>  HTTP/1.1 200 OK                  HEADERS
>  Content-Type: image/jpeg   ==>     - END_STREAM
>  Transfer-Encoding: chunked         + END_HEADERS
>  Trailer: Foo                         :status = 200
>                                       content-length = 123
>  123                                  content-type = image/jpeg
>  {binary data}                        trailer = Foo
>  0
>  Foo: bar                         DATA
>                                     - END_STREAM
>                                   {binary data}
> 
>                                   HEADERS
>                                     + END_STREAM
>                                     + END_HEADERS
>                                       foo = bar
> ```
