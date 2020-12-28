# HTTP

### TCP/IP

* HTTP, DNS
* TCP (3次握手：1. SYN 2. SYN/ACK 3. ACK)
* IP (IP Address, MAC Address (硬件地址) 多次中转)
* 链路层

### HTTP 方法

* GET, POST, PUT, HEAD, DELETE, OPTIONS, TRACE, CONNECT

### HTTP 持久连接

* 只要认意一端没有明确提出断开连接，则保持TCP连接状态
* 管线化（pipelining）：同时并行发送多个请求

### 状态管理: HTTP + Cookie

* 在请求和响应报文中写入Cookie信息

---

### HTTP 报文

* 报文（message）：HTTP通信的基本单位，8bit字节流组成（octet sequence）
* 实体（entity）
* 压缩传输的内容编码
* 分块传输编码（Chunked Transfer Coding）
* 多部份对象集合（Multiparty）
  * multipart/form-data
  * multipart/byteranges （206 Partial Content）
  * boundary (--)
* 范围请求（Range Request）
* 内容协商（Content Negotiation）

---

### HTTP 状态码

|      | 类别          | 原因短语                   |
| ---- | ------------- | -------------------------- |
| 1XX  | Informational | 接收的请求正在处理         |
| 2XX  | Success       | 请求正常处理完毕           |
| 3XX  | Redirection   | 需要进行附加操作以完成请求 |
| 4XX  | Client Error  | 服务器无法处理请求         |
| 5XX  | Server Error  | 服务器处理请求出错         |

* 200 OK
* 204 No Content
* 206 Partial Content
* 301 Moved Permanently
* 302 Found (临时性重定向)
* 303 See Other (类似302，但要求客户端使用GET)
  * 301，302，303的处理方法类似303
* 304 Not Modified (客户端发送的附带条件未满足)
* 307 Temporary Redirect (类似302，但更多浏览器会符合标准，即不强制转换POST至GET)
* 400 Bad Request
* 401 Unauthorized (第一次响应返回401会包含用以质询的用户信息，第二次返回401说明用户认证失败)
* 403 Forbidden
* 404 Not Found
* 500 Internal Server Error
* 503 Service Unavailable
* 状态码和实际状况可能不一致

---

