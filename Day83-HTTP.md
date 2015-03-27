#HTTP

参考文章：

[全世界的屋顶：HTTP 请求（GET与POST区别）和响应](http://www.blogjava.net/honeybee/articles/164008.html)

[HTTP协议请求消息与应答消息（get,post）抓包详解](http://baiyan425.blog.51cto.com/1573961/614737)

[小坦克: HTTP详解](http://www.cnblogs.com/tankxiao/archive/2012/02/13/2342672.html)

##HTTP历史

**超文本传输协议(HTTP)是一种通信协议，它允许将超文标记语言(HTML)文档从web服务器传送到客户端的浏览器。**

HTTP 有三个版本，分别是：

- HTTP 0.9
- HTTP 1.0
- HTTP 1.1

现在广泛使用的是 HTTP 1.1 版本。

>HTTP 1.1的新特性是支持部分内容请求/响应，这意味着当客户端请求的数据量很大时，可以分多次发起请求，每次请求只要求获取整块数据的一部分。Web服务器也可以分多次响应，每次只返回整块数据的一部分。这使得流媒体得以实现。

>从HTTP 1.1开始，客户端默认与Web 服务器建立长连接，这种连接适合Web上数据量较大的丰富应用，使得资源消耗更少。


##URL 
HTTP 协议通过URL (统一资源定位符)来访问资源。一个完整的 URL 组成如下：

```URL
http://myname:mypass@www.vimer.cn:80/mydir/myfile.html?myvar=myvalue#myfrag
```
- `http`:协议名称
- `myname`: 用户名(可选)
- `mypass`: 密码(可选)
- `www.vimer.cn`: 主机网络地址
- `80`: 端口号
- `/mydir/myfile.html`: 资源路径
- `myvar=myvalue`: 查询字符串(可选)，发给HTTP服务器的数据
- `myfrag`: 锚点(可选)

可见协议名称用 `://` 结束，用户名和密码以 `:` 分隔，以 `@` 结束，端口号与主机网络地址以 `:` 分隔，资源路径与查询字符串以 `?` 分隔，锚点以 `#` 开头。

只有协议名称，主机网络地址和资源路径是必须包含在 URL 里的。一个常见的例子如下：

```
http://www.google.com/
```
这里面，http协议，www.google.com 是主义网络地址，末尾的是资源路径，我们平时用浏览器的时候值输入 `www.google.com` 是浏览器为我们补全了前面的 `http://` 和后面的 `/`。浏览器发起请求时用的 URL 格式还是完整的，WEB 浏览器并不强制用户输入格式规范的 URL。

##HTTP 请求与响应

#####GET: 

向特定的资源发出请求。注意：GET方法不应当被用于产生“副作用”的操作中，例如在Web 应用程序中。其中一个原因是GET可能会被网络蜘蛛等随意访问。

#####POST：

向指定资源提交数据进行处理请求（例如提交表单或者上传文件）。数据被包含在请求体中。POST请求可能会导致新的资源的建立和/或已有资源的修改。

##HTTP 请求格式

```HTTP
<request line>
<headers>
<blank line>
[<request_body>]
```

在 HTTP 请求中，第一行必须是一个请求行(request line) ，用来说明请求类型、要访问的资源以及使用的 HTTP 版本。紧接着是一个首部(header)小节，用来说明服务器要使用的附加信息。在首部之后是一个空行，再此之后可以天剑任意的其他数据[称之为主体(body)]。

##HTTP响应格式

```HTTP
<status line>
<headers>
<blank line>
[<response_body>]
```

在响应中唯一真正的区别在于第一行中用状态信息代替了请求信息。状态行（status line）通过提供一个状态码来说明所请求的资源情况。