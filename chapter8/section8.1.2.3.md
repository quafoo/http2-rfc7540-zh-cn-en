# Request Pseudo-Header Fields / 请求的伪首部字段
> The following pseudo-header fields are defined for HTTP/2 requests:
> 
> * The :method pseudo-header field includes the HTTP method ([*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231), [Section 4](http://httpwg.org/specs/rfc7231.html#methods)).
> 
> * The :scheme pseudo-header field includes the scheme portion of the target URI ([*[RFC3986]*](http://httpwg.org/specs/rfc7540.html#RFC3986), [Section 3.1](https://tools.ietf.org/html/rfc3986#section-3.1)).
> 
>  :scheme is not restricted to http and https schemed URIs. A proxy or gateway can translate requests for non-HTTP schemes, enabling the use of HTTP to interact with non-HTTP services.
> 
> * The :authority pseudo-header field includes the authority portion of the target URI ([*[RFC3986]*](http://httpwg.org/specs/rfc7540.html#RFC3986), [Section 3.2](https://tools.ietf.org/html/rfc3986#section-3.2)). The authority MUST NOT include the deprecated userinfo subcomponent for http or https schemed URIs.
> 
> 	To ensure that the HTTP/1.1 request line can be reproduced accurately, this pseudo-header field MUST be omitted when translating from an HTTP/1.1 request that has a request target in origin or asterisk form (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 5.3](http://httpwg.org/specs/rfc7230.html#request-target)). Clients that generate HTTP/2 requests directly SHOULD use the :authority pseudo-header field instead of the Host header field. An intermediary that converts an HTTP/2 request to HTTP/1.1 MUST create a Host header field if one is not present in a request by copying the value of the :authority pseudo-header field.
> 
> * The :path pseudo-header field includes the path and query parts of the target URI (the path-absolute production and optionally a '?' character followed by the query production (see Sections [3.3](https://tools.ietf.org/html/rfc3986#section-3.3) and [3.4](https://tools.ietf.org/html/rfc3986#section-3.4) of [*[RFC3986]*](http://httpwg.org/specs/rfc7540.html#RFC3986)). A request in asterisk form includes the value '*' for the :path pseudo-header field.
> 
> 	This pseudo-header field MUST NOT be empty for http or https URIs; http or https URIs that do not contain a path component MUST include a value of '/'. The exception to this rule is an OPTIONS request for an http or https URI that does not include a path component; these MUST include a :path pseudo-header field with a value of '\*' (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 5.3.4](http://httpwg.org/specs/rfc7230.html#asterisk-form)).

HTTP/2请求定义了如下伪首部字段：

* `:method` 伪首部字段包含HTTP方法( [*[RFC7231]*](http://httpwg.org/specs/rfc7540.html#RFC7231)，[第4章](http://httpwg.org/specs/rfc7231.html#methods) )。
* `:scheme` 伪首部字段包含目标URI( [*[RFC3986]*](http://httpwg.org/specs/rfc7540.html#RFC3986)，[3.1节](https://tools.ietf.org/html/rfc3986#section-3.1) )的方案部分。

  `:scheme`并不局限于http和https URIs方案。代理或网关可以转化非HTTP方案的请求，从而可以使用HTTP和非HTTP的服务进行交互。

* `:authority`伪首部字段包含目标URI( [*[RFC3986]*](http://httpwg.org/specs/rfc7540.html#RFC3986)，[3.2节](https://tools.ietf.org/html/rfc3986#section-3.2) )的authority部分。authority不能包含http或https URIs方案废弃的用户信息的子组件。

  为了确保HTTP/1.1请求队列可以被精确地复制，当转换具有源或星号形式(参见 [ *[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [5.3节](http://httpwg.org/specs/rfc7230.html#request-target) )请求目标的HTTP/1.1请求时，该伪首部字段必须被忽略。直接生成HTTP/2请求的客户端应该使用`:authority`伪首部字段代替Host首部字段。中介将HTTP/2请求转换为HTTP/1.1请求时，如果通过拷贝`:authority`伪首部字段的值，请求里没有出现Host首部字段，就必须创建一个Host首部字段。

* `:path`伪首部字段包含目标URI的path和query部分(绝对路径部分和一个后跟query部分的可选的'?'字符(参见 [*[RFC3986]*](http://httpwg.org/specs/rfc7540.html#RFC3986) 的 [3.3节](https://tools.ietf.org/html/rfc3986#section-3.3) 和 [3.4节](https://tools.ietf.org/html/rfc3986#section-3.4) ))。星号形式的请求包含:path伪首部字段的'*'值。

  对于http或者https URIs，该伪首部字段不能为空。不包含path组件的http或https URIs必须包含一个'/'值。该规则的例外情况是OPTIONS请求，其http或https URI不包含path组件，此时必须包含一个值为'\*'的`:path`伪首部字段(参见 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)， [5.3.4节](http://httpwg.org/specs/rfc7230.html#asterisk-form) )。


> All HTTP/2 requests MUST include exactly one valid value for the :method, :scheme, and :path pseudo-header fields, unless it is a CONNECT request ([Section 8.3](http://httpwg.org/specs/rfc7540.html#CONNECT)). An HTTP request that omits mandatory pseudo-header fields is malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

除非是CONNECT请求( [8.3节](http://httpwg.org/specs/rfc7540.html#CONNECT) )，否则对于`:method`、`:scheme`和`:path`伪首部字段，所有的HTTP/2请求都必须只包含一个有效值。忽略了强制的伪首部字段的HTTP请求是有缺陷的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。


> HTTP/2 does not define a way to carry the version identifier that is included in the HTTP/1.1 request line.

HTTP/2没有定义携带HTTP/1.1请求行里包含的版本号的方法。