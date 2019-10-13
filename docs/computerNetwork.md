# 计算机网络基础
#### 操作系统里如何调度
进程调度方式：<br/>
**非抢占式**<br/>
一旦处理机分配给某个进程后，就一直让其运行直到完成或者发生某事件而阻塞（如`I/O`请求）<br/>
**抢占式**<br/>
允许调度程序根据某种规则，暂停某个正在执行的进程，由以下规则：优先权原则：优先级高的抢占当前进程；短进程优先原则；时间片原则：各进程按时间片轮转运行时，当正在执行的进程的时间片用完后，便停止该进程的执行而重新进行调度。<br/>
#### 进程和线程的区别
进程是资源分配的最小单位，线程是程序执行的最小单位。<br/>
进程有自己的独立地址空间，每启动一个进程，系统就会为它分配地址空间，建立数据表来维护代码段、堆栈段和数据段，这种操作非常昂贵。而线程是共享进程中的数据的，使用相同的地址空间，因此CPU切换一个线程的花费远比进程要小很多，同时创建一个线程的开销也比进程要小很多。<br/>
线程之间的通信更方便，同一进程下的线程共享全局变量、静态变量等数据，而进程之间的通信需要以通信的方式`(IPC)`进行。不过如何处理好同步与互斥是编写多线程程序的难点。<br/>
但是多进程程序更健壮，多线程程序只要有一个线程死掉，整个进程也死掉了，而一个进程死掉并不会对另外一个进程造成影响，因为进程有自己独立的地址空间。
#### 计算机网络模型分层
 | 分层名称 | 功能概述 | 常见协议
-|-|-|-
7 | 应用层 | 针对特定应用的协议 | `FTP、DNS、HTTP、SMTP`等
6 | 表示层 | 负责数据格式的转换 | 不用协议
5 | 会话层 | 负责建立和断开通信连接 | 不用协议
4 | 传输层 | 负责可靠的数据传输 | `TCP、UDP`等
3 | 网络层 | 负责将数据传输到目标地址 | `ARP、IP、RARP`等
2 | 数据链路层 | 负责物理层面的互连，节点之间的通信传输 | `802.11、WIFI`等
1 | 物理层 | 负责物理电路的比特流互换传输 | `RS-443`等
 
#### 什么是`TCP/IP`
`TCP/IP` 指传输控制协议/网际协议 `(Transmission Control Protocol / Internet Protocol)`,是供已连接因特网的计算机进行通信的通信协议。定义了电子设备（比如计算机）如何连入因特网，以及数据如何在它们之间传输的标准。

#### `TCP`三次握手
1、客户端发送`syn`包到服务器，等待服务器确认接收。<br/>
2、服务器确认接收`syn`包并确认客户的`syn`，并发送回来一个`syn+ack`的包给客户端。<br/>
3、客户端确认接收服务器的`syn+ack`包，并向服务器发送确认包`ack`，二者相互建立联系后，完成`tcp`三次握手。

#### `TCP`四次挥手
1、客户端进程发出连接释放报文，并且停止发送数据。<br/>
2、服务器收到连接释放报文，发出确认报文，此时，服务端就进入了`CLOSE-WAIT`（关闭等待）状态。<br/>
3、客户端收到服务器的确认请求后，此时，客户端就进入`FIN-WAIT-2`（终止等待2）状态，等待服务器发送连接释放报文。<br/>
4、服务器将最后的数据发送完毕后，就向客户端发送连接释放报文，此时，服务器就进入了`LAST-ACK`（最后确认）状态，等待客户端的确认。<br/>
5、客户端收到服务器的连接释放报文后，必须发出确认，此时，客户端就进入了`TIME-WAIT`（时间等待）状态。注意此时`TCP`连接还没有释放，必须经过`2∗∗MSL`（最长报文段寿命）的时间后，当客户端撤销相应的`TCB`后，才进入`CLOSED`状态。<br/>
6、服务器只要收到了客户端发出的确认，立即进入`CLOSED`状态。同样，撤销`TCB`后，就结束了这次的`TCP`连接。可以看到，服务器结束`TCP`连接的时间要比客户端早一些。

#### `TCP`与`UDP`的区别
`TCP`（`Transmission Control Protocol`，传输控制协议）提供的是面向连接，可靠的字节流服务。即客户和服务器交换数据前，必须现在双方之间建立一个`TCP`连接，之后才能传输数据。并且提供超时重发，丢弃重复数据，检验数据，流量控制等功能，保证数据能从一端传到另一端。<br/>
`UDP`（`User Data Protocol`，用户数据报协议）是一个简单的面向数据报的运输层协议。它不提供可靠性，只是把应用程序传给IP层的数据报发送出去，但是不能保证它们能到达目的地。由于UDP在传输数据报前不用再客户和服务器之间建立一个连接，且没有超时重发等机制，所以传输速度很快。<br/><br/>
1、`TCP`面向连接（如打电话要先拨号建立连接）;`UDP`是无连接的，即发送数据之前不需要建立连接。<br/>
2、`TCP`提供可靠的服务。也就是说，通过`TCP`连接传送的数据，无差错，不丢失，不重复，且按序到达;`UDP`尽最大努力交付，即不保证可靠交付。<br/>
3、`TCP`面向字节流，实际上是`TCP`把数据看成一连串无结构的字节流;`UDP`是面向报文的。`UDP`没有拥塞控制，因此网络出现拥塞不会使源主机的发送速率降低（对实时应用很有用，如IP电话，实时视频会议等）<br/>
4、每一条`TCP`连接只能是点到点的;`UDP`支持一对一，一对多，多对一和多对多的交互通信。<br/>
5、`TCP`首部开销`20`字节;`UDP`的首部开销小，只有`8`个字节。<br/>
6、`TCP`的逻辑通信信道是全双工的可靠信道，`UDP`则是不可靠信道。

#### `Socket`
`socket`是一组实现`TCP/UDP`通信的接口`API`，既无论`TCP`还是`UDP`，通过对`scoket`的编程，都可以实现`TCP/UDP`通信。<br/>
`socket`作为一个通信链的句柄，它包含了网络通信必备的`5`种信息:<br/>
1、连接使用的协议。<br/>
2、本地主机的`IP`地址。<br/>
3、本地进程的协议端口。<br/>
4、远程主机的`IP`地址。<br/>
5、远程进程的协议端口。

#### `URL(Uniform Resource Locator)`统一资源定位符
`URL`的语法规则： `scheme：//host/domain:port/path/filename`<br/>
1、`scheme`：定义因特网服务的类型，最常见的有`http`。<br/>
2、`host`：定义域主机（`http`默认主机是`www`）。<br/>
3、`domain`：定义因特网域名，比如`“www.baidu.com”`。<br/>
4、`path`：定义服务器上的路径。<br/>
5、`filename`：资源名

#### `DNS(Domain Name Server)`域名服务器
在浏览器输入域名后的解析过程:<br/>
1、浏览器根据地址去本身缓存中查找`dns`解析记录，如果有，则直接返回IP地址，否则浏览器会查找操作系统中（`hosts`文件）是否有该域名的`dns`解析记录，如果有则返回。<br/>
2、如果浏览器缓存和操作系统`hosts`中均无该域名的`dns`解析记录，或者已经过期，此时就会向域名服务器发起请求来解析这个域名。<br/>
3、请求会先到`LDNS`（本地域名服务器），让它来尝试解析这个域名，如果LDNS也解析不了，则直接到根域名解析器请求解析。<br/>
4、根域名服务器给`LDNS`返回一个所查询的主域名服务器`（gTLDServer）`地址。<br/>
5、此时`LDNS`再向上一步返回的`gTLD`服务器发起解析请求。<br/>
6、`gTLD`服务器接收到解析请求后查找并返回此域名对应的`Name Server`域名服务器的地址，这个`Name Server`通常就是你注册的域名服务器（比如阿里`dns`、腾讯`dns`等）。<br/>
7、`Name Server`域名服务器会查询存储的域名和IP的映射关系表，正常情况下都根据域名得到目标`IP`记录，连同一个`TTL`值返回给`DNS Server`域名服务器。<br/>
8、返回该域名对应的`IP`和`TTL`值，`Local DNS Server`会缓存这个域名和`IP`的对应关系，缓存的时间有`TTL`值控制。<br/>
9、把解析的结果返回给用户，用户根据`TTL`值缓存在本地系统缓存中，域名解析过程结束。

#### 用户输入`URL`到浏览器显现给用户页面经过了什么过程
1、用户输入`URL`，浏览器获取到`URL`<br/>
2、浏览器(应用层)进行`DNS`解析（直接输入`IP`地址既跳过该步骤）<br/>
3、根据解析出的`IP`地址+端口，浏览器（应用层）发起`HTTP`请求，请求中携带（请求头`header`、请求体`body`）<br/>
4、请求到达传输层，`tcp`协议为传输报文提供可靠的字节流传输服务，它通过三次握手等手段来保证传输过程中的安全可靠。通过对大块数据的分割成一个个报文段的方式提供给大量数据的便携传输。<br/>
5、到网络层， 网络层通过`ARP`寻址得到接收方的`Mac`地址，`IP`协议把在传输层被分割成一个个数据包传送接收方。<br/>
6、数据到达数据链路层，请求阶段完成<br/>
7、接收方在数据链路层收到数据包之后，层层传递到应用层，接收方应用程序就获得到请求报文。<br/>
8、接收方收到发送方的`HTTP`请求之后，进行请求文件资源（如`HTML`页面）的寻找并响应报文<br/>
9、发送方收到响应报文后，如果报文中的状态码表示请求成功，则接受返回的资源（如`HTML`文件），进行页面渲染。

#### `HTTP`状态码
`2XX` 成功<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`200 OK`: 表示从客户端发来的请求在服务器端被正确处理<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`204 No content`: 表示请求成功，但响应报文不含实体的主体部分<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`206 Partial Content`: 进行范围请求<br/>
`3XX` 重定向<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`301 moved permanently`: 永久性重定向，表示资源已被分配了新的 `URL`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`302 found`，临时性重定向: 表示资源临时被分配了新的 `URL`<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`303 see other`: 表示资源存在着另一个 `URL`，应使用 `GET` 方法定向获取资源<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`304 not modified`: 表示服务器允许访问资源，但因发生请求未满足条件的情况<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`307 temporary redirect`: 临时重定向，和`302`含义相同<br/>
`4XX` 客户端错误<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`400 bad request`: 请求报文存在语法错误<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`401 unauthorized`: 表示发送的请求需要有通过 `HTTP` 认证的认证信息<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`403 forbidden`: 表示对请求资源的访问被服务器拒绝<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`404 not found`: 表示在服务器上没有找到请求的资源<br/>
`5XX` 服务器错误<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`500 internal sever error`: 表示服务器端在执行请求时发生了错误<br/>
&nbsp;&nbsp;&nbsp;&nbsp;`503 service unavailable`: 表明服务器暂时处于超负载或正在停机维护，无法处理请求<br/>
#### `HTTP`协议格式
`HTTP`的请求和响应的消息协议是一样的，分为三个部分，起始行、消息头和消息体。这三个部分以`CRLF`作为分隔符。最后一个消息头有两个`CRLF`，用来表示消息头部的结束。<br/>
消息头部有很多键值对组成，多个键值对之间使用`CRLF`作为分隔符，也可以完全没有键值对。形如`Content-Encoding: gzip`
消息体是一个字符串，字符串的长度是由消息头部的`Content-Length`键指定的。如果没有`Content-Length`字段说明没有消息体，譬如`GET`请求就是没有消息体的，`POST`请求的消息体一般用来放置表单数据。`GET`请求的响应返回的页面内容也是放在消息体里面的。我们平时调用`API`返回的`JSON`内容都是放在消息体里面的。
#### `HTTP`的无状态性？
所谓`HTTP`协议的无状态性是指服务器的协议层无需为不同的请求之间建立任何相关关系，它特指的是协议层的无状态性。但是这并不代表建立在`HTTP`协议之上的应用程序就无法维持状态。应用层可以通过会话`Session`来跟踪用户请求之间的相关性，服务器会为每个会话对象绑定一个唯一的会话`ID`，浏览器可以将会话`ID`记录在本地缓存`LocalStorage`或者`Cookie`，在后续的请求都带上这个会话`ID`，服务器就可以为每个请求找到相应的会话状态。
#### 浏览器如何渲染页面
1、浏览器通过`HTML Parser`根据深度遍历的原则把HTML解析成`DOM Tree`。<br/>
2、将`CSS`解析成 `CSS Rule Tree (CSSOM Tree)`。<br/>
3、根据`DOM`树和`CSSOM`树来构造`render Tree`。<br/>
4、`layout`: 根据得到的`render Tree`来计算所有节点的位置。<br/>
5、`paint`: 遍历`render` 树，并调用硬件图形`API`来绘制每个节点。














