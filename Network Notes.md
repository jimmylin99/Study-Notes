---
typora-copy-images-to: img
---

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
  * 一般伴随`WWW-Authenticate`首部
  * `WWW-Authenticate: <type> realm=<realm>`
    * `<type>`: Authentication type, e.g. `Basic`
    * `<realm>`: 一个保护区域的描述，如未指定，客户端通常显示一个格式化的主机名来替代
* 403 Forbidden
* 404 Not Found
* 500 Internal Server Error
* 503 Service Unavailable
* 状态码和实际状况可能不一致

---

### HTTP 首部 (HTTP Header)

##### Authorization

`Authorization: <type> <credentials>`

`<type>`:

* `Basic`: `Base64`编码，需要与`HTTPS`一起使用，否则等效于明文传输
* `Bearer`
* `Digest`
* `HOBA`
* `Mutual`
* `AWS4-HMAC-SHA256`

---

# HTTPS

由于HTTP是（1. 明文传输；2. 无法验证身份；3. 无法验证内容完整性），为了安全，额外使用SSL (Secure Socket Layer) 和 TLS (Transport Layer Security) 协议

HTTP原本是直接与TCP协议通信，现在中间还需要经过SSL/TLS（TLS是基于SSL3.0发展的，下文统称两者为SSL，SSL3.0及其以前版本均有安全漏洞，通常TLS1.0被标识为SSL3.1，TLS1.1->SSL3.2，TLS1.2->SSL3.3 defined in Aug. 2008，TLS1.3 defined in Aug. 2018）

HTTPS默认使用443端口，从而方便指定使用HTTPS而不是HTTP；另外，也可以发出请求指定协议，从而在服务器端进行跳转

### SSL/TLS

[Wikipedia provides a good start](https://en.wikipedia.org/wiki/Transport_Layer_Security)

SSL/TLS是在TCP/IP协议组中属于应用层协议（but not neatly fit into OSI model or TCP/IP model, it runs on top of some reliable transport protocol e.g. TCP; see [wikipedia](https://en.wikipedia.org/wiki/Transport_Layer_Security#Description))。首先(HTTP协议中，发出请求的始终是客户端，产生响应的是服务器)，客户端发出握手请求，SSL会先建立加密通信，使用非对称加密和数字证书的方式建立安全连接；随后，将对称加密所用密钥传输，以便后续通信使用。这种混合加密方式，主要是因为非对称加密更耗时，而对称加密无法保证密钥安全地传输到对方，而采取的这种办法。

###### 非对称加密

公钥加密，私钥解密：经典如RSA，会在服务器端（HTTP协议语境下）生成一对公钥和私钥，公钥发送给客户端，客户端即可使用公钥加密明文，服务器收到加密内容后，使用私钥解密。

* 因此，非对称加密使得明文从客户端到服务器端的单向传输变得安全
* 相比于对称加密仅使用同样的密钥，非对称加密如果想实现双向安全传输，需要双方均生成一对公钥和私钥，在传输过程中交替扮演服务器的角色（思想类似于HTTP，服务器的扮演者是动态的）

私钥加密，公钥解密：如进行数字摘要，由发信人在Hash后使用私钥加密生成数字签名，附在原文后一并发送；收信人用公钥解密数字签名并用同样的Hash算法检验摘要（Digest, or say HMAC -- Hashed Message Authentication Code）的一致性

* HMAC-X: `X` stands for the hashing algorithm, such as HMAC-SHA256

非对称加密和对称加密较容易理解，但是即使使用非对称加密，如何保证传输的接受或发送对象（客户端和服务器）不被黑客伪装，是一个主要问题，解决方案是使用数字证书。

###### 数字证书

1. 服务器将公钥和基本信息（如域名）发送给第三方证书认证权威机构（CA），CA在验证申请者真实身份后，用私钥加密生成数字证书

2. 当服务器与客户端通信时，不再直接传递公钥，而是用数字证书代替

3. **（？尚不明确）**客户端只处理收信任的第三方权威机构并验证该证书是否由期望的服务器发送而来（通过CA的公钥解密数字证书来验证），在解密数字证书后获得服务器的公钥，至此客户端成功验证服务器的身份

   * 对于服务器验证客户端身份，原理一致，方向相反，但由于数字证书的申请并非免费，对于大量的客户端而言，很少会使用数字证书，常见的使用场景如银行客户的线上银行账号需要使用数字证书来验证客户端身份
   * 受信任的第三方权威机构是重点，如果黑客自行给自己颁发证书，客户端如果信任该证书颁发者，那么数字证书就无法抵抗中间人攻击，形同虚设；因此客户端对于CA的信任十分重要，一般而言这些受信任的CA会随着浏览器的发布被一同添加到受信任列表里

   More about digital signature: [理解数字证书及制作过程 - (rootdeep.github.io)](https://rootdeep.github.io/posts/ssl/)

一些关于攻击https的问题：

1. 如果证书被劫持（很容易，因为证书是公开的），并且把公钥替换成黑客自行生成的公钥（黑客有自己生成的密钥了），不就可以伪装了吗？
   * naive 证书有hash值，改变公钥，hash会变，进一步改变hash，由于整个证书是CA用它自己的密钥加密的，内容变了（无论是公钥还是hash），当客户端用CA的公钥解密（一般浏览器都会有可信CA的公钥），得到的会是乱码
   * 小结：证书内容不可变，因为证书的数字摘要用的是CA的私钥
2. 如果冒充服务器向CA申请证书呢？
   * 几乎不可以，CA会验证域名对应的信息，包括管理邮箱验证
3. 如果证书被劫持（easy），我也不改变公钥，我就装作是服务器发给客户端呢？
   * 客户端可能会信你，但是那又如何，你又不知道私钥，所以客户端通过CA里的公钥加密后的session key又不能被你解密，session key如果不对，后面的对称加密和解密得到的也会是乱码

所以https还是很安全的，但是如果客户端将一个假的CA（黑客自行创建的）添加到受信任列表，那就没办法了，所以不要信任任何不知名的CA（知名的CA都会随浏览器等软件一起发行）

客户端（浏览器）因数字证书问题而导致的可能错误提示：

1. "Not Trusted for this Website": possibly hijacked by hacker; the certificate is self-signed

2. "Your connection is not secure (or private)": possibly the certificate is expired or revoked

3. "Mixed Content": some of the components of HTTPS websites are retrieved over HTTP, to solve this, change the URLs starting with `http://` to `https://`

4. "Name Mismatch": the domain name is wrong (e.g. certificate requested for `domain.org` instead of `domain.com`), to solve this one can use `Subject Alternative Name (SAN) Certificate` to cover hundreds of different domains (e.g. `*.domain.com`)

   More about SAN: [What is a SAN Certificate? - SSL.com](https://www.ssl.com/faqs/what-is-a-san-certificate/#)

More about error message due to digital certificate: [Avoid common HTTPS errors - SSL.com](https://www.ssl.com/article/avoid-common-https-errors/)



---

# SSH

1. `host` generate public and private key
2. `host` send public key to `server` (need password to login so far; *TODO*: any other methods?)

Under Windows, we do

1. `ssh-keygen -t rsa -b 2048`

2. `type $env:USERPROFILE\.ssh\id_rsa.pub | ssh {IP-ADDRESS-OR-FQDN} "cat >> .ssh/authorized_keys"`

* Reference: [Windows 10 OpenSSH Equivalent of "ssh-copy-id" – Christopher Hart - IT Adventures – Documenting my discoveries in the IT world (chrisjhart.com)](https://www.chrisjhart.com/Windows-10-ssh-copy-id/)

Under Unix, we do 

1. `ssh-keygen -t rsa -b 2048` (same as windows)
2. `ssh-copy-id id@server`

* Reference: [How to automate SSH login with password? - Server Fault](https://serverfault.com/questions/241588/how-to-automate-ssh-login-with-password)

---

# Cryptography

### Hashing Algorithms

* MD5, SHA-1: can deliberately find collisions by brute force methods
* SHA-2 (usually used SHA-256): no collision found so far

* 彩虹表攻击：攻击者记录了一张巨大的密码库，预先计算了常用密码的 hash 值，这样只需要搜索 hash 值就能寻找到一个合适的密码用于登录。
* 解决彩虹表的问题是加盐，在加密之前，对原文混入其他信息，混入的信息不存放到数据库中。实际寻找到其他原文也无法登录。（原文指比如hash前的密码明文）
* 另一种方法：通过算法快速的找到具有相同hash值的大量原文，这样不依赖彩虹表就可以实施攻击。当被攻击者价值非常大，攻击者获取足够多的撞库原文，还是能分析盐值。

* `Bcrypt`: hash值可以不一样（调整加盐参数，hash重复杂凑次数，存储的时候把这些参数一并存储即可）；计算非常缓慢

  More: [Bcrypt加密之新认识 - 简书 (jianshu.com)](https://www.jianshu.com/p/2b131bfc2f10)

* `Scrypt`: 未做更多了解，应该是bcrypt的加强版，但bcrypt的安全性已经较高

* 建议保证密码位数和复杂度

* `Spring Security`有相关class

  More: [密码安全存储——PBKDF2、bcrypt、scrypt - ZANGZHIYUAN - 博客园 (cnblogs.com)](https://www.cnblogs.com/gm-201705/p/9863918.html)

# 端口

[计算机网络端口详解 - 简书](https://www.jianshu.com/p/d8a601323670)

###### 什么是端口

> 在Internet上，各主机间通过TCP/TP协议发送和接收数据报，各个数据报根据其目的主机的IP地址来进行互联网络中的路由选择。可见，把数据报顺利的传送到目的主机是没有问题的。我们知道大多数操作系统都支持多程序（进程）同时运行，那么目的主机应该把接收到的数据报传送给众多同时运行的进程中的哪一个？显然这个问题有待解决，端口机制便由此被引入了进来。
>
> 本地操作系统会给那些有需求的进程分配协议端口（protocal port，即我们常说的端口），每个协议端口由一个正整数标识，如：80，139等等。当目的主机接收到数据报后，将根据报文首部的目的端口号，把数据发送到相应端口，而与此端口相对应的那个进程将会领取数据并等待下一组数据的到来。
>
> 端口其实就是队，操作系统为各个进程分配了不同的队，数据报按照目的端口被推入相应的队中，等待被进程取用，在极特殊的情况下，这个队也是有可能溢出的，不过操作系统允许各进程指定和调整自己的队的大小。不光接受数据报的进程需要开启它自己的端口，发送数据报的进程也需要开启端口，这样，数据报中将会标识有源端口，以便接受方能顺利的回传数据报到这个端口。

```bash
netstat -an
netstat -rn
lsof -i
lsof -i :port_number
```

同一个端口可以被多个协议使用（其实主要就是传输层的TCP和UDP）

# UDP

User Datagram Protocol 即用户数据包协议

位于传输层的协议，无状态连接，从应用层收到什么就发什么，不考虑数据包是否安全完整地到达，可能会丢包，但速度快

# TCP

Transmission Control Protocol 即传输控制协议

三次握手：SYN; SYN+ACK; ACK 保证接收端也知晓自己的序列号被发送端接收到

四次挥手：FIN+ACK; ACK; FIN; ACK





# IP 地址

IP地址由net-id和host-id，即网络号和主机号组成

> IP-address ::= { <Network-ID>, <Host-ID> }

地址分类

A类（主机ID有3个字节）：1-126 B类：128-191 C类（主机ID仅一个字节)：192-223 D类（用于multicast，一对多通信）：224-239 E类（保留，为以后使用）：240-255

特殊IP地址

> 1）{0,0}：网络号和主机号都全部为0，表示“本网络上的本主机”，只能用作源地址；
> 2）{0，host-id}：本网络上的某台主机。只能用作源地址；
> 3）{-1,-1}：表示网络号和主机号的所有位上都是1（二进制），用于本网络上的广播，只能用作目的地址，发到该地址的数据包不能转发到源地址所在网络之外；
> 4）{net-id,-1}：直接广播到指定的网络上。只能用作目的地址；
> 5）{net-id,subnet-id,-1}：直接广播到指定网络的指定子网络上。只用作目的地址；
> 6）{net-id,-1,-1}：直接广播到指定网络的所有子网络上。只能用作目的地址；
> 7）{127，}：即网络号为127的任意ip地址。都是内部主机回环地址(loopback),永远都不能出现在主机外部的网络中。

More: [脑残式网络编程入门(八)：你真的了解127.0.0.1和0.0.0.0的区别？ - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/110577979)

### `127.0.0.1` vs `0.0.0.0` vs `localhost`

127.0.0.1

- 1）当一台主机还没有被分配一个IP地址的时候，用于表示主机本身（DHCP分配IP地址的时候）；
- 2）用作默认路由，表示”任意IPV4主机”；
- 3）用来表示目标机器不可用；
- 4）用作服务端，表示本机上的任意IPV4地址

127.0.0.1

- 1）回环测试：通过使用ping 127.0.0.1 测试某台机器上的网络设备，操作系统或者TCP/IP实现是否工作正常；
- 2）DDos攻击防御：网站收到DDos攻击之后，将域名A记录到127.0.0.1，即让攻击者自己攻击自己；
- 3）程序测试：大部分Web容器测试的时候绑定的本机地址

**题外知识：127.0.0.1与localhost的关系**

相比127.0.0.1，localhost具有更多的意义。localhost是个域名，而不是一个ip地址。之所以我们经常把localhost与127.0.0.1认为是同一个是因为我们使用的大多数电脑上都讲localhost指向了127.0.0.1这个地址。

在ubuntu系统中，/ets/hosts文件中都会有如下内容：

> 127.0.0.1 localhost
> 127.0.1.1 52im-aliyun
> \# The following lines are desirable for IPv6 capable hosts
> ::1 ip6-localhost ip6-loopback
> fe00::0 ip6-localnet
> ff00::0 ip6-mcastprefix
> ff02::1 ip6-allnodes
> ff02::2 ip6-allrouters

上面第一行是几乎每台电脑上都会有的默认配置。

但是localhost的意义并不局限于127.0.0.1。

localhost是一个域名，用于指代this computer或者this host，可以用它来获取运行在本机上的网络服务。

在大多数系统中，localhost被指向了IPV4的127.0.0.1和IPV6的::1：

> 127.0.0.1 localhost
> ::1 localhost

所以，在使用的时候要注意确认IPV4还是IPV6

**在实际应用中：**一般我们在服务端绑定端口的时候可以选择绑定到0.0.0.0，这样我的服务访问方就可以通过我的多个ip地址访问我的服务

# WSGI

Web Server Gateway Interface 网关服务器接口，用于python，java中有servlet规范，下文主要讨论python下的WSGI

More 全面: [WSGI接口 - 廖雪峰的官方网站 (liaoxuefeng.com)](https://www.liaoxuefeng.com/wiki/897692888725344/923057027806560)

More: [浅谈Java web开发和Python web开发区别 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/46294999)

为了让底层代码由专门的服务器软件实现，而我们用Python专注于生成HTML文档。因为我们不希望接触到TCP连接、HTTP原始请求和响应格式，所以，需要一个统一的接口，让我们专心用Python编写Web业务。

这个接口就是WSGI：Web Server Gateway Interface。

WSGI接口的定义：

* 一个简单的函数

  ```python
  def application(environ, start_response):
      start_response('200 OK', [('Content-Type', 'text/html')])
      return '<h1>Hello, web!</h1>'
  ```

  上面的`application()`函数就是符合WSGI标准的一个HTTP处理函数，它接收两个参数：

  - environ：一个包含所有HTTP请求信息的`dict`对象；
  - start_response：一个发送HTTP响应的函数。

总结，WSGI将TCP/IP协议中的大部分工作完成，只暴露两个参数和一个返回值，再复杂的python web应用也是基于这个接口设计的（除非手写底层处理代码，即抛弃WSGI），