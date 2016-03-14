# Examples / 示例
> This section shows HTTP/1.1 requests and responses, with illustrations of equivalent HTTP/2 requests and responses.

本节展示了HTTP/1.1请求和响应，及其等价的HTTP/2请求和响应实例。


> An HTTP GET request includes request header fields and no payload body and is therefore transmitted as a single [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames containing the serialized block of request header fields. The [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame in the following has both the END_HEADERS and END\_STREAM flags set; no [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames are sent.

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

HTTP GET请求包含请求首部字段，没有负载体，因此可以通过一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧来传输，后跟零个或多个包含序列化请求首部块的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧。下面的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧包含END\_HEADERS和END\_STREAM标志，没有发送 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧。

```
 GET /resource HTTP/1.1           HEADERS
 Host: example.org          ==>     + END_STREAM
 Accept: image/jpeg                 + END_HEADERS
                                      :method = GET
                                      :scheme = https
                                      :path = /resource
                                      host = example.org
                                      accept = image/jpeg
```


> Similarly, a response that includes only response header fields is transmitted as a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame (again, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames) containing the serialized block of response header fields.

> ```
>  HTTP/1.1 304 Not Modified        HEADERS
>  ETag: "xyzzy"              ==>     + END_STREAM
>  Expires: Thu, 23 Jan ...           + END_HEADERS
>                                       :status = 304
>                                       etag = "xyzzy"
>                                       expires = Thu, 23 Jan ...
> ```

类似地，只包含响应首部字段的响应通过一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧(同样，后跟零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧)来传输，其包含序列化的响应首部块。

```
 HTTP/1.1 304 Not Modified        HEADERS
 ETag: "xyzzy"              ==>     + END_STREAM
 Expires: Thu, 23 Jan ...           + END_HEADERS
                                      :status = 304
                                      etag = "xyzzy"
                                      expires = Thu, 23 Jan ...
```


> An HTTP POST request that includes request header fields and payload data is transmitted as one [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames containing the request header fields, followed by one or more [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames, with the last [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) (or [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS)) frame having the END_HEADERS flag set and the final [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frame having the END\_STREAM flag set:

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

包含请求首部字段和负载数据的HTTP POST请求通过一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧传输，后跟零个或多个包含请求首部字段的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧，后跟一个或多个 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧，最后的 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧(或者 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧)设置了END\_HEADERS标志，最后的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧设置了END\_STREAM标志：

```
 POST /resource HTTP/1.1          HEADERS
 Host: example.org          ==>     - END_STREAM
 Content-Type: image/jpeg           - END_HEADERS
 Content-Length: 123                  :method = POST
                                      :path = /resource
 {binary data}                        :scheme = https

                                  CONTINUATION
                                    + END_HEADERS
                                      content-type = image/jpeg
                                      host = example.org
                                      content-length = 123

                                  DATA
                                    + END_STREAM
                                  {binary data}
```


> Note that data contributing to any given header field could be spread between header block fragments. The allocation of header fields to frames in this example is illustrative only.

注意，组成任何给定首部字段的数据可以在首部块片段之间散布。本例帧中首部字段的分配仅仅是示例性的。


> A response that includes header fields and payload data is transmitted as a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames, followed by one or more [DATA](http://httpwg.org/specs/rfc7540.html#HEADERS) frames, with the last [DATA](http://httpwg.org/specs/rfc7540.html#HEADERS) frame in the sequence having the END_STREAM flag set:

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


包含首部字段和负载数据的响应通过一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧传输，后跟零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧，后跟一个或多个 [DATA](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧，序列的最后一个 [DATA](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧设置了END\_STREAM标志：

```
 HTTP/1.1 200 OK                  HEADERS
 Content-Type: image/jpeg   ==>     - END_STREAM
 Content-Length: 123                + END_HEADERS
                                      :status = 200
 {binary data}                        content-type = image/jpeg
                                      content-length = 123

                                  DATA
                                   + END_STREAM
                                  {binary data}
```


> An informational response using a 1xx status code other than 101 is transmitted as a [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame, followed by zero or more [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) frames.

一个使用除101之外的1xx状态码的信息性响应通过一个 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧传输，后跟零个或多个 [CONTINUATION](http://httpwg.org/specs/rfc7540.html#CONTINUATION) 帧。


> Trailing header fields are sent as a header block after both the request or response header block and all the [DATA](http://httpwg.org/specs/rfc7540.html#DATA) frames have been sent. The [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) frame starting the trailers header block has the END_STREAM flag set.

请求或响应和所有的 [DATA](http://httpwg.org/specs/rfc7540.html#DATA) 帧被发送完之后，拖尾的首部字段被当做一个首部块发送。开始拖尾首部块的 [HEADERS](http://httpwg.org/specs/rfc7540.html#HEADERS) 帧设置了 END\_STREAM 标志。


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

如果请求的Expect首部字段里包含一个"100-continue"标识，那么应在其响应里发送一个100(Continue)状态码，下面的示例就包括了一个这样的状态码，还包括了拖尾的首部字段：

```
 HTTP/1.1 100 Continue            HEADERS
 Extension-Field: bar       ==>     - END_STREAM
                                    + END_HEADERS
                                      :status = 100
                                      extension-field = bar

 HTTP/1.1 200 OK                  HEADERS
 Content-Type: image/jpeg   ==>     - END_STREAM
 Transfer-Encoding: chunked         + END_HEADERS
 Trailer: Foo                         :status = 200
                                      content-length = 123
 123                                  content-type = image/jpeg
 {binary data}                        trailer = Foo
 0
 Foo: bar                         DATA
                                   - END_STREAM
                                  {binary data}

                                  HEADERS
                                    + END_STREAM
                                    + END_HEADERS
                                      foo = bar
```