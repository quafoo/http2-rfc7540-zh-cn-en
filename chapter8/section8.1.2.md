# HTTP Header Fields / HTTP首部字段
> HTTP header fields carry information as a series of key-value pairs. For a listing of registered HTTP headers, see the "Message Header Field" registry maintained at <https://www.iana.org/assignments/message-headers>.

HTTP首部字段以一系列键值对的形式携带信息。登记的HTTP首部列表，可以参考在 <https://www.iana.org/assignments/message-headers> 处维护的"Message Header Field"记录。


> Just as in HTTP/1.x, header field names are strings of ASCII characters that are compared in a case-insensitive fashion. However, header field names MUST be converted to lowercase prior to their encoding in HTTP/2. A request or response containing uppercase header field names MUST be treated as malformed ([Section 8.1.2.6](http://httpwg.org/specs/rfc7540.html#malformed)).

像在HTTP/1.x中一样，首部字段名称是ASCII字符串，不区分大小写。但是，在被HTTP/2编码之前，必须将首部字段名称转换成小写的。必须将包含大写首部字段名称的请求或响应看做是畸形的( [8.1.2.6节](http://httpwg.org/specs/rfc7540.html#malformed) )。