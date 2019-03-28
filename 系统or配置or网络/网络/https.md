原文：https://www.cloudxns.net/Support/detail/id/2465.html

- pic: https://ooo.0o0.ooo/2016/11/28/583bd8e8eb523.jpg

握手的目的简单概括就是：通信双方协商出一套会话密钥，然后基于此密钥通过对称加密方式来安全通信。

Client Hello
握手交互流程，首先由 Client 端发起 ClientHello 请求。在这个请求中 Client 端向 Server 端提供如下信息：

SSL version : 自己支持的最高协议版本，比如 TLS1.2。

Ciphers : 支持的加密套件，比如： RSA 非对称加密算法，AES 对称加密算法。

Random number: Client 端随机数，将会用来生成会话密钥。
Server Hello
Server 收到 ClientHello 请求后需要回应一系列内容，从 ServerHello 到 ServerHelloDone，有些服务端的实现是每条单独发送，有些服务端实现是合并到一起发送。

ServerHello
根据 Client 端的请求信息确认使用的协议版本和加密套件（Cipher Suite），和客户端一致，并生成一个 Server 端随机数，用来生成会话密钥。

Certificate
Server 端用于证明自身身份的凭证，一种由专门的数字证书认证机构（Certificate Authority 简称 CA）通过非常严格的审核之后颁发的电子证书，由 Client 端去认证 Server 端的合法身份。

ServerKeyExchange
可选的，补充生成会话密钥的信息。对于前面协商的有些加密算法若 Certificate 未提供足够的信息或就没有 Certificate 那么需要发送该消息。进一步的细节我们就不深入了，可以查看参考[1]。

CertificateRequest
可选的，Server 端需要认证 Client 端身份的请求时发送。比如，银行提供的各类 U 盾，其实就是一种 Client 证书，一般在线使用专业版网银时才需要。

ServerHelloDone
表示 Server 响应结束。

Client Finished
Client 收到 Server 回应后，首先验证 Server 的证书。如果证书不是可信机构颁布、或者证书中的域名与实际域名不一致、或者证书已经过期，就会向访问者显示一个警告，由其选择是否还要继续通信。如果证书没有问题，Client 会继续回应 Server，包括如下内容：

Certificate
Client 端响应 Server 的 CertificateRequest 请求。

ClientKeyExchange Client 再生成一个随机数，又称 premaster secret 用于生成会话密钥的信息，并把这个随机数传递给 Server 用于 Server 生成相同的会话密钥。

CertificateVerify
用于对客户端证书提供证明，对于特定的证书需要可选发送。

ChangeCipherSpec
用于告知 Server，Client 已经切换到之前协商好的加密套件（Cipher Suite）的状态，准备使用之前协商好的加密套件和会话密钥加密数据并传输了。

Finished
Client 会使用之前协商好的加密套件和会话密钥加密一段 Finished 的数据传送给 Server，此数据是为了在正式传输应用数据之前对刚刚握手建立起来的加解密通道进行验证。

Server Finished
Server 在接收到客户端传过来的 premaster secret 数据之后，也会使用跟 Client 同样的方式生成会话密钥。一切就绪后，服务端回应如下内容：

NewSessionTicket
表示新建了一个会话票据，传递给 Client。若连接意外中断 Client 需要重建会话时，可以复用该票据，加速握手过程。

ChangeCipherSpec
告知 Client 已经切换到协商过的加密套件状态，准备使用加密套件和会话密钥加密数据并传输了。

Finished
Server 会使用之前协商好的加密套件和会话密钥加密一段 Finished 的数据传送给 Client，此数据是为了在正式传输应用数据之前对刚刚握手建立起来的加解密通道进行验证。

至此，整个握手阶段全部结束。接下来 Client 和 Server 进入使用会话密钥的加密通信过程。

认证
前面提及证书验证部分属于 SSL/TLS 协议中比较复杂的部分，我们单独用一节分析下。证书是由 CA 签发的，所以要验证证书的有效性需要去 CA 的服务器，流程如下。

详细见原文
