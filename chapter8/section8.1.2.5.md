# Compressing the Cookie Header Field / 压缩Cookie首部字段
> The [Cookie header field](http://httpwg.org/specs/rfc7540.html#COOKIE) [COOKIE] uses a semi-colon (";") to delimit cookie-pairs (or "crumbs"). This header field doesn't follow the list construction rules in HTTP (see [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230), [Section 3.2.2](http://httpwg.org/specs/rfc7230.html#field.order)), which prevents cookie-pairs from being separated into different name-value pairs. This can significantly reduce compression efficiency as individual cookie-pairs are updated.

[Cookie首部字段](http://httpwg.org/specs/rfc7540.html#COOKIE) *[COOKIE]* 使用分号来分隔cookie对(或『面包屑』)。该首部字段没有遵循HTTP里的列表创建规则(参见 [*[RFC7230]*](http://httpwg.org/specs/rfc7540.html#RFC7230)，[3.2.2节](http://httpwg.org/specs/rfc7230.html#field.order) )，这阻止了将cookie对分隔为不同的`名-值`对。这会极大地降低cookie对更新时的压缩效率。


> To allow for better compression efficiency, the Cookie header field MAY be split into separate header fields, each with one or more cookie-pairs. If there are multiple Cookie header fields after decompression, these MUST be concatenated into a single octet string using the two-octet delimiter of 0x3B, 0x20 (the ASCII string "; ") before being passed into a non-HTTP/2 context, such as an HTTP/1.1 connection, or a generic HTTP server application.

为了更好的压缩效率，可以将Cookie首部字段拆分成单独的首部字段，每一个都有一个或多个cookie对。如果解压缩后有多个Cookie首部字段，在将其传入一个非HTTP/2的上下文(比如：HTTP/1.1连接，或者通用的HTTP服务器应用)之前，必须使用两个字节的分隔符0x3B，0x20(即ASCII字符串"; ")将这些Cookie首部字段连接成一个字符串。


> Therefore, the following two lists of Cookie header fields are semantically equivalent.
> 
> ```
> cookie: a=b; c=d; e=f
> 
> cookie: a=b
> cookie: c=d
> cookie: e=f
> ```

因此，以下两个Cookie首部字段列表在语义上是等价的：

```
cookie: a=b; c=d; e=f

cookie: a=b
cookie: c=d
cookie: e=f
```
