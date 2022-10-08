# 浏览器相关
#### 浏览器如何渲染页面
1、浏览器通过`HTML Parser`根据深度遍历的原则把HTML解析成`DOM Tree`。<br/>
2、将`CSS`解析成 `CSS Rule Tree (CSSOM Tree)`。<br/>
3、根据`DOM`树和`CSSOM`树来构造`render Tree`。<br/>
4、`layout`: 根据得到的`render Tree`来计算所有节点的位置。<br/>
5、`paint`: 遍历`render` 树，并调用硬件图形`API`来绘制每个节点。

#### 浏览器的重排、重绘
当`render tree`中的一部分(或全部)因为元素的规模尺寸，布局，隐藏等改变而需要重新构建。这就称为回流`(reflow)`;<br>
当`render tree`中的一些元素需要更新属性，而这些属性只是影响元素的外观，风格，而不会影响布局的，比如`background-color`。则就叫称为重绘;<br>
回流必将引起重绘，而重绘不一定会引起回流。比如：只有颜色改变的时候就只会发生重绘而不会引起回流<br>
当页面布局和几何属性改变时就需要回流 比如：添加或者删除可见的DOM元素，元素位置改变，元素尺寸改变——边距、填充、边框、宽度和高度，内容改变

`layout`：也叫`reflow`重排，渲染中的一种行为。当`render tree`中任一节点的几何尺寸发生改变了，`render tree`都会重新布局。<br>
`repaint`：重绘，渲染中的一种行为。`render tree`中任一元素样式属性（几何尺寸没改变）发生改变了，`render tree`都会重新画，比如字体颜色、背景等变化。
#### 介绍一下你对浏览器内核的理解？
主要分成两部分： 渲染引擎和`js`引擎<br/>
渲染引擎：负责取得网页的内容（`HTML、XML`、图像等等）、整理讯息（例如加入`CSS`等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。<br/>
`JS`引擎：解析和执行`javascript`来实现网页的动态效果。<br/>

常见内核<br/>
`Trident`内核：`IE,MaxThon,TT,The World,360`,搜狗浏览器等。<br/>
`Gecko`内核：`Netscape6`及以上版本，`FF,MozillaSuite/SeaMonkey`等。<br/>
`Presto`内核：`Opera7`及以上。<br/>
`Webkit`内核：`Safari,Chrome`等。

#### 关于XML，你了解哪些？
设计宗旨：XML 被设计为传输和存储数据，其焦点是数据的内容；HTML 被设计用来显示数据，其焦点是数据的外观。HTML 旨在显示信息，而 XML 旨在传输信息。<br/>

作用：<br/>
作为项目或者模板的配置文件<br/>
作为网络传输数据的格式(现在已JSON为主)<br/>
用来保存数据,而且这些数据具有自我描述性<br/>

#### `cookie`属性值
一个`Cookie`包含以下信息：<br>
1、`name`: `Cookie`名称，`Cookie`名称必须使用只能用在`URL`中的字符，一般用字母及数字，不能包含特殊字符，如有特殊字符想要转码。如`js`操作`cookie`的时候可以使用`escape()`对名称转码。<br>
2、`value`: `Cookie`值，`Cookie`值同理`Cookie`的名称，可以进行转码和加密。<br>
3、`expire`: `Expires`，过期日期，一个`GMT`格式的时间，当过了这个日期之后，浏览器就会将这个`Cookie`删除掉，当不设置这个的时候，`Cookie`在浏览器关闭后消失。<br>
4、`path`: `Path`，一个路径，在这个路径下面的页面才可以访问该`Cookie`，一般设为`“/”`，以表示同一个站点的所有页面都可以访问这个`Cookie`。<br>
5、`Domain`: 子域，指定在该子域下才可以访问`Cookie`，例如要让`Cookie`在`a.test.com`下可以访问，但在`b.test.com`下不能访问，则可将`domain`设置成`a.test.com`。<br>
6、`Secure`: 安全性，指定`Cookie`是否只能通过`https`协议访问，一般的`Cookie`使用`HTTP`协议既可访问，如果设置了`Secure`（没有值），则只有当使用`https`协议连接时`cookie`才可以被页面访问。<br>
#### `cookie、session、sessionStorage、localStorage`理解和区别
1、`cookie`是网站为了标示用户身份而储存在用户本地的数据（通常经过加密）。 `cookie`数据始终在同源的`http`请求中携带（即使不需要），也会在浏览器和服务器间来回传递。<br/>
2、`session`和`cookie`的作用有点类似，都是为了存储用户相关的信息。服务器使用`session`把用户的信息临时保存在了服务器上，用户离开网站后`session`会被销毁。不同的是，`cookie`是存储在本地浏览器，而`session`存储在服务器。存储在服务器的数据会更加的安全，不容易被窃取。`cookie`数据大小不能超过`4k`。 `session`大小没有限制。<br/>
一般有两种存储方式：<br/>
1)、存储在服务端：通过`cookie`存储一个`session_id`，然后具体的数据则是保存在`session`中。如果用户已经登录，则服务器会在`cookie`中保存一个`session_id`，下次再次请求的时候，会把该`session_id`携带上来，服务器根据`session_id`在`session`库中获取用户的`session`数据。就能知道该用户到底是谁，以及之前保存的一些状态信息。这种专业术语叫做`server side session`。<br/>
2)、将`session`数据加密，然后存储在`cookie`中。这种专业术语叫做`client side session`。`flask`采用的就是这种方式，但是也可以替换成其他形式。<br/>

3、`sessionStorage`：将数据保存在`session`对象中。所谓`session`，是指用户在浏览某个网站时，从进入网站到浏览器关闭所经过的这段时间，也就是用户浏览这个网站所花费的时间。`session`对象可以用来保存在这段时间内所要求保存的任何数据。<br/>
4、`localStorage`：将数据保存在客户端本地的硬件设备(通常指硬盘，也可以是其他硬件设备)中，即使浏览器被关闭了，该数据仍然存在，下次打开浏览器访问网站时仍然可以继续使用。<br/>
这两者的区别在于，`sessionStorage`为临时保存，而`localStorage`为永久保存。`sessionStorage`和`localStorage` 虽然也有存储大小的限制，但比`cookie`大得多，可以达到`5M`或更大。<br/>
有期时间： `localStorage` 存储持久数据，浏览器关闭后数据不丢失除非主动删除数据； `sessionStorage` 数据在当前浏览器窗口关闭后自动删除。 `cookie` 设置的`cookie`过期时间之前一直有效，即使窗口或浏览器关闭。
#### 什么是http-only？这个是干什么用的？
`http-only`能够有效地防止`XSS`攻击。<br/>
1、什么是`http-only`？<br/>
`HttpOnly`是包含在`http`返回头`Set-Cookie`里面的一个附加的`flag`，所以它是后端服务器对`cookie`设置的一个附加的属性，在生成`cookie`时使用`HttpOnly`标志有助于减轻客户端脚本访问受保护`cookie`的风险（如果浏览器支持的话）
通过`js`脚本将无法读取到c`ookie`信息，这样能有效的防止`XSS`攻击。<br/>

2、这个是干什么用的？其他相关点!<br/>
大多数`XSS`攻击都是针对会话`cookie`的盗窃。后端服务器可以通过在其创建的`cookie`上设置`HttpOnly`标志来帮助缓解此问题，这表明该`cookie`在客户端上不可访问。<br/>
如果支持`HttpOnly`的浏览器检测到包含`HttpOnly`标志的`cookie`，并且客户端脚本代码尝试读取该`cookie`，则浏览器将返回一个空字符串作为结果。这会通过阻止恶意代码（通常是XSS）将数据发送到攻击者的网站来使攻击失败。<br/>

#### 前后端交互的数据交换格式是什么？
json、xml、form表单

#### 如何实现浏览器内多个标签页之间的通信?
`WebSocket、SharedWorker`。 也可以调用`localstorge、cookies`等本地存储方式。<br/>
`localStorage`另一个浏览上下文里被添加、修改或删除时，它都会触发一个事件， 我们通过监听事件，控制它的值来进行页面信息通信。<br/>
监听`localStorage`事件:
```js
window.onstorage = (e) => {console.log(e)}
// 或者这样
window.addEventListener('storage', (e) => console.log(e))
```
`onstorage`以及`storage`事件，针对都是非当前页面对`localStorage`进行修改时才会触发，当前页面修改`localStorage`不会触发监听函数。<br/>
然后就是在对原有的数据的值进行修改时才会触发，比如原本已经有一个`key`会`a`值为`b`的`localStorage`，你再执行：`localStorage.setItem('a', 'b')`代码，同样是不会触发监听函数的。<br/>

`shareWorker`
```js
// 这段代码是必须的，打开页面后注册SharedWorker，显示指定worker.port.start()方法建立与worker间的连接
if (typeof Worker === "undefined") {
  alert('当前浏览器不支持webworker')
} else {
  let worker = new SharedWorker('worker.js')
  worker.port.addEventListener('message', (e) => {
    console.log('来自worker的数据：', e.data)
  }, false)
  worker.port.start()
  window.worker = worker
}
// 获取和发送消息都是调用postMessage方法，我这里约定的是传递'get'表示获取数据。
window.worker.port.postMessage('get')
window.worker.port.postMessage('发送信息给worker')
```
页面`A`发送数据给`worker`，然后打开页面`B`，调用`window.worker.port.postMessage('get')`，即可收到页面`A`发送给`worker`的数据。
#### `HTML5`的离线储存怎么使用，工作原理能不能解释一下？
在用户没有与因特网连接时，可以正常访问站点或应用，在用户与因特网连接时，更新用户机器上的缓存文件。<br/>
原理：`HTML5`的离线存储是基于一个新建的`.appcache`文件的缓存机制(不是存储技术)，通过这个文件上的解析清单离线存储资源，这些资源就会像`cookie`一样被存储了下来。之后当网络在处于离线状态下时，浏览器会通过被离线存储的数据进行页面展示。
#### 浏览器是怎么对`HTML5`的离线储存资源进行管理和加载的呢？
在线的情况下，浏览器发现`html`头部有`manifest`属性，它会请求`manifest`文件，如果是第一次访问`app`，那么浏览器就会根据`manifest`文件的内容下载相应的资源并且进行离线存储。如果已经访问过`app`并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的`manifest`文件与旧的`manifest`文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。离线的情况下，浏览器就直接使用离线存储的资源。

#### 什么是事件代理/事件委托？
事件代理/事件委托是利用事件冒泡的特性，将本应该绑定在多个元素上的事件绑定在他们的祖先元素上，尤其在动态添加子元素的时候，可以非常方便的提高程序性能，减小内存空间。
#### 什么是事件冒泡？什么是事件捕获？
冒泡型事件：事件按照从最特定的事件目标到最不特定的事件目标(`document`对象)的顺序触发。<br/>
捕获型事件：事件从最不精确的对象(`document` 对象)开始触发，然后到最精确。<br/>

支持`W3C`标准的浏览器在添加事件时用`addEventListener(event,fn,useCapture)`方法，基中第`3`个参数`useCapture`是一个`Boolean`值，用来设置事件是在事件捕获时执行，还是事件冒泡时执行。而不兼容`W3C`的浏览器`(IE)`用`attachEvent()`方法，此方法没有相关设置，不过`IE`的事件模型默认是在事件冒泡时执行的，也就是在`useCapture`等于`false`的时候执行，所以把在处理事件时把`useCapture`设置为`false`是比较安全，也实现兼容浏览器的效果。
#### 如何阻止冒泡？
`w3c`的方法是`e.stopPropagation()`，IE则是使用`e.cancelBubble = true`。例如： 
`window.event? window.event.cancelBubble = true : e.stopPropagation();`
`return false`也可以阻止冒泡。
#### 如何阻止默认事件？
`w3c`的方法是`e.preventDefault()`，`IE`则是使用`e.returnValue = false`，比如：
```js
function stopDefault( e ) { 
    //阻止默认浏览器动作(W3C) 
    if ( e && e.preventDefault ) 
        e.preventDefault(); 
    //IE中阻止函数器默认动作的方式 
    else 
        window.event.returnValue = false; 
}
//return false也能阻止默认行为。
```
#### 原生对象和宿主对象
原生对象是`ECMAScript`规定的对象，所有内置对象都是原生对象，比如`Array、Date、RegExp`等；<br/>
宿主对象是宿主环境比如浏览器规定的对象，用于完善是`ECMAScript`的执行环境，比如`Document、Location、Navigator`等。

#### 请尽可能详尽的解释`AJAX`的优缺点
1、最大的一点是页面无刷新，在页面内与服务器通信，给用户的体验非常好。<br/>
2、使用异步方式与服务器通信，不需要打断用户的操作，具有更加迅速的响应能力。<br/>
3、可以把以前一些服务器负担的工作转嫁到客户端，利用客户端闲置的能力来处理，减轻服务器和带宽的负担，节约空间和宽带租用成本，`ajax`的原则是“按需取数据”，可以最大程度的减少冗余请求。<br/>
4、基于标准化的并被广泛支持的技术，不需要下载插件或者小程序。<br/>

`ajax`的缺点:<br/>
`ajax`对浏览器后退机制造成了破坏；安全问题；对搜索引擎的支持比较弱；破坏了程序的异常机制
#### `get`和`post`有什么区别？
其实，`GET`和`POST`本质上两者没有任何区别。他们都是`HTTP`协议中的请求方法。底层实现都是基于`TCP/IP`协议。所谓区别，只是浏览器厂家根据约定，做得限制而已。<br/>
`get`是通过明文发送数据请求，而`post`是通过密文；<br/>
`get`传输的数据量有限，因为`url`的长度有限，`post`则不受限；<br/>
`GET`请求的参数只能是`ASCII`码，所以中文需要`URL`编码，而`POST`请求传参没有这个限制<br/>
`GET`产生一个`TCP`数据包；`POST`产生两个`TCP`数据包。<br/>
对于`GET`方式的请求，浏览器会把`http header`和`data`一并发送出去，服务器响应`200`（返回数据）；而对于`POST`，浏览器先发送`header`，服务器响应`100 continue`，浏览器再发送`data`，服务器响应`200 ok`（返回数据）。
#### 请指出`document.onload`和`document.ready`两个事件的区别
页面加载完成有两种事件，一是`ready`，表示文档结构已经加载完成（不包含图片等非文字媒体文件），二是`onload`，指示页面包含图片等文件在内的所有元素都加载完成。
#### 同源策略
同源策略是浏览器的一个安全功能，不同源的客户端脚本在没有明确授权的情况下，不能读写对方资源。<br>
相同的协议`(protocol)`、主机(域名)、端口(如果指定)的两个页面是属于同一个源`<script> <img> <iframe>`中的`src`，`href`都可以任意链接网络资源，是不遵循通源策略的。
#### 请解释`JSONP`的工作原理，以及它为什么不是真正的`AJAX`
`JSONP (JSON with Padding)`是一个简单高效的跨域方式，`HTML`中的`script`标签可以加载并执行其他域的`javascript`，于是我们可以通过`script`标记来动态加载其他域的资源。<br/>
`AJAX`是不跨域的，而`JSONP`是一个是跨域的，还有就是二者接收参数形式不一样！

#### 实现JSONP
```js
function fn () {
  let script = document.createElement("script");
  script.setAttribute('src', 'https://api.douban.com/v2/book/search?q=javascript&count=1&callback=handleResponse')
  document.querySelector('body').append(script);
}

function handleResponse() {
  console.log(9999)
}
```

#### http请求方法
http请求中的8种请求方法<br/>
1、options 返回服务器针对特定资源所支持的HTML请求方法或web服务器发送*测试服务器功能（允许客户端查看服务器性能）<br/>
2、Get 向特定资源发出请求（请求指定页面信息，并返回实体主体）<br/>
3、Post 向指定资源提交数据进行处理请求（提交表单、上传文件），又可能导致新的资源的建立或原有资源的修改<br/>
4、Put  向指定资源位置上上传其最新内容（从客户端向服务器传送的数据取代指定文档的内容）<br/>
5、Head  与服务器索与get请求一致的相应，响应体不会返回，获取包含在小消息头中的原信息（与get请求类似，返回的响应中没有具体内容，用于获取报头）<br/>
6、Delete  请求服务器删除request-URL所标示的资源*（请求服务器删除页面）<br/>
7、Trace   回显服务器收到的请求，用于测试和诊断<br/>
8、Connect   HTTP/1.1协议中能够将连接改为管道方式的代理服务器<br/>

#### 处理跨域的方式
`JSONP`<br>
`CORS`<br>
`WebSocket`<br>
`postMessage`<br>
`nginx` 反向代理
#### 为什么操作`DOM`会很慢
由于`DOM`和`JavaScript`是被分开独立实现的，因此，每一次在通过`js`操作`DOM`的时候，就需要先去连接`js`和`DOM`，我们可以这样理解：把`DOM`和`JavaScript`比作两个岛，他们之间通过一个收费的桥连接着，每一次访问`DOM`的时候，就需要经过这座桥，并且给“过路费”，访问的次数越多，路费就会越高，并且访问到`DOM`后，操作具体的`DOM`还需要给“操作费”，由于浏览器访问`DOM`的操作很多，因此，“路费”和“操作费”自然会增加，这就是为什么操作`DOM`会很慢的原因。
#### 事件循环
**EventLoop**<br>
`js`中的运行机制，来解决单线程运行带来的一些问题。`js`是一种单线程语言，所有任务都在一个线程下完成，一旦遇到大量任务或者遇到一个耗时的任务，网页就会出现假死的状态，因为`JavaScript`停不下来，也就无法响应用户的行为。<br>
`EventLoop`是一个程序结构，用于等待和发送消息和事件。<br>
简单说，就是在程序中设置两个线程：一个负责程序本身的运行，称为"主线程"；另一个负责主线程与其他进程（主要是各种`I/O`操作）的通信，被称为"`EventLoop`线程"（可以译为"消息线程"）<br>

**执行栈与任务队列**<br>
因为`js`是单线程语言，当遇到异步任务(如`ajax`操作等)时，不可能一直等待异步完成，再继续往下执行，在这期间浏览器是空闲状态，显而易见这会导致巨大的资源浪费。
执行栈: 当执行某个函数、用户点击一次鼠标，`Ajax`完成，一个图片加载完成等事件发生时，只要指定过回调函数，这些事件发生时就会进入任务队列中，等待主线程读取,遵循先进先出原则。执行任务队列中的某个任务，这个被执行的任务就称为执行栈。

**主线程**<br>
主线程循环：即主线程会不停的从执行栈中读取事件，会执行完所有栈中的同步代码。<br>
当遇到一个异步事件后，并不会一直等待异步事件返回结果，而是会将这个事件挂在与执行栈不同的队列中，我们称之为任务队列(`Task Queue`)。<br>
当主线程将执行栈中所有的代码执行完之后，主线程将会去查看任务队列是否有任务。如果有，那么主线程会依次执行那些任务队列中的回调函数。

**js 异步执行的运行机制**<br>
1、所有任务都在主线程上执行，形成一个执行栈。<br>
2、主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。<br>
3、一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"。那些对应的异步任务，结束等待状态，进入执行栈并开始执行。<br>
4、主线程不断重复上面的第三步。<br>

**宏任务与微任务**<br>
异步任务分为 宏任务（`macrotask`） 与 微任务 (`microtask`)，不同的`API`注册的任务会依次进入自身对应的队列中，然后等待 `Event Loop` 将它们依次压入执行栈中执行。<br>

**Event Loop(事件循环)**<br>
执行栈选择最先进入队列的宏任务(通常是`script`整体代码)，如果有则执行<br>
检查是否存在 `Microtask`，如果存在则不停的执行，直至清空 `microtask` 队列<br>
更新`render`(每一次事件循环，浏览器都可能会去更新渲染)<br>
重复以上步骤<br>

<img width="500" src="/Frontend-Interview-Question/images/eventLoop.png" />

1、将所有任务看成两个队列：执行队列与事件队列。<br>
2、执行队列是同步的，事件队列是异步的，宏任务放入事件列表，微任务放入执行队列之后，事件队列之前。<br>
3、当执行完同步代码之后，就会执行位于执行列表之后的微任务，然后再执行事件列表中的宏任务
#### 浏览器优化
现代浏览器大多都是通过队列机制来批量更新布局，浏览器会把修改操作放在队列中，至少一个浏览器刷新（即`16.6ms`）才会清空队列，但当你获取布局信息的时候，队列中可能有会影响这些属性或方法返回值的操作，即使没有，浏览器也会强制清空队列，触发回流与重绘来确保返回正确的值。

主要包括以下属性或方法：<br>
`offsetTop、offsetLeft、offsetWidth、offsetHeight scrollTop、scrollLeft、scrollWidth、scrollHeight clientTop、clientLeft、clientWidth、clientHeight width、height getComputedStyle() getBoundingClientRect()`<br>
所以，我们应该避免频繁的使用上述的属性，他们都会强制渲染刷新队列。
#### 减少重绘与回流
使用 `transform` 替代 `top`<br>
使用 `visibility` 替换 `display: none` ，因为前者只会引起重绘，后者会引发回流（改变了布局 避免使用`table`布局，可能很小的一个小改动会造成整个 `table` 的重新布局。<br>
尽可能在`DOM`树的最末端改变`class`，回流是不可避免的，但可以减少其影响。尽可能在`DOM`树的最末端改变`class`，可以限制了回流的范围，使其影响尽可能少的节点。<br>
避免设置多层内联样式，`CSS` 选择符从右往左匹配查找，避免节点层级过多。<br>
将动画效果应用到`position`属性为`absolute`或`fixed`的元素上<br>
避免使用`CSS`表达式，可能会引发回流。<br>
`CSS3` 硬件加速（`GPU`加速）
#### 浏览器和`Node`事件循环的区别
其中一个主要的区别在于浏览器的`event loop` 和`nodejs`的`event loop` 在处理异步事件的顺序是不同的,`nodejs`中有`micro event`;其中`Promise`属于`micro event` 该异步事件的处理顺序就和浏览器不同。`nodejs V11.0`以上 这两者之间的顺序就相同了。

`Node 10`以前：<br>
执行完一个阶段的所有任务<br>
执行完`nextTick`队列里面的内容<br>
然后执行完微任务队列的内容<br>

`Node 11`以后：<br>
和浏览器的行为统一了，都是每执行一个宏任务就执行完微任务队列。
```js
function test () {
   console.log('start')
    setTimeout(() => {
        console.log('children2')
        Promise.resolve().then(() => {console.log('children2-1')})
    }, 0)
    setTimeout(() => {
        console.log('children3')
        Promise.resolve().then(() => {console.log('children3-1')})
    }, 0)
    Promise.resolve().then(() => {console.log('children1')})
    console.log('end') 
}

test()

// 以上代码在node11以下版本的执行结果(先执行所有的宏任务，再执行微任务)
// start
// end
// children1
// children2
// children3
// children2-1
// children3-1

// 以上代码在node11及浏览器的执行结果(顺序执行宏任务和微任务)
// start
// end
// children1
// children2
// children2-1
// children3
// children3-1
```
#### `cookie` 和 `token` 都存放在 `header` 中，为什么不会劫持 `token`？
题目可能有点问题，在劫持面前，不管`cookie`还有`token`，都能劫持。<br>
只是说： `cookie`会自动携带上，而`token`需要设置`header`才可。<br>
具体说一下`xss`层面的劫持和`csxf`层面的劫持：<br>
`xss`: 劫持`cookie`或者`localStorage`，从而伪造用户身份相关信息。前端层面`token`会存在哪儿？不外乎`cookie localStorage` sessionStorage,这些东西都是通过js代码获取到的。<br>
解决方案：过滤标签`<>`,不信任用户输入， 对用户身份等`cookie`层面的信息进行`http-only`处理。<br>

`csxf`：是后端过于乐观的将`header`区的`cookie`取到（所以这才是主要原因，不是因为会自动携带`cookie`所以不安全，是后端代码不安全而已），并当作用户信息进行相关操作。解决方案也很简单，对于`cookie`不信任，对每次请求都进行身份验证，比如`token`的处理。
#### `Virtual DOM` 真的比操作原生 `DOM` 快吗？谈谈你的想法。
没有任何框架可以比纯手动的优化 `DOM` 操作更快，因为框架的 `DOM` 操作层需要应对任何上层 `API` 可能产生的操作，它的实现必须是普适的。在构建一个实际应用的时候，你难道为每一个地方都去做手动优化吗？出于可维护性的考虑，这显然不可能。框架给你的保证是，你在不需要手动优化的情况下，我依然可以给你提供过得去的性能。
#### 可以分成 `Service Worker`、`Memory Cache`、`Disk Cache` 和 `Push Cache`，那请求的时候 `from memory cache` 和 `from disk cache` 的依据是什么，哪些数据什么时候存放在 `Memory Cache` 和 `Disk Cache`中？
如果开启了`Service Worker`首先会从`Service Worker`中拿。<br>
如果新开一个以前打开过的页面缓存会从`Disk Cache`中拿（前提是命中强缓存）<br>
刷新当前页面时浏览器会根据当前运行环境内存来决定是从 `Memory Cache` 还是从`Disk Cache`中拿<br>
对于大文件来说，大概率是不存储在内存中的，反之优先<br>
当前系统内存使用率高的话，文件优先存储进硬盘

#### 浏览器缓存
浏览器缓存分为强缓存和协商缓存。当客户端请求某个资源时，获取缓存的流程如下<br/>
先根据这个资源的一些 http header 判断它是否命中强缓存，如果命中，则直接从本地获取缓存资源，不会发请求到服务器； 当强缓存没有命中时，客户端会发送请求到服务器，服务器通过另一些 request header验证这个资源是否命中协商缓存，称为 http 再验证，如果命中，服务器将请求返回，但不返回资源，而是告诉客户端直接从缓存中获取，客户端收到返回后就会从缓存中获取资源； 强缓存和协商缓存共同之处在于，如果命中缓存，服务器都不会返回资源； 区别是，强缓存不对发送请求到服务器，但协商缓存会。 当协商缓存也没命中时，服务器就会将资源发送回客户端。 当 ctrl+f5 强制刷新网页时，直接从服务器加载，跳过强缓存和协商缓存； 当 f5 刷新网页时，跳过强缓存，但是会检查协商缓存；

#### 介绍下前端加密的常见场景和方法
首先，加密的目的，简而言之就是将明文转换为密文、甚至转换为其他的东西，用来隐藏明文内容本身，防止其他人直接获取到敏感明文信息、或者提高其他人获取到明文信息的难度。

前端密码传输过程中如果不加密，在日志中就可以拿到用户的明文密码，对用户安全不太负责。 这种加密其实相对比较简单，可以使用 `PlanA`-前端加密、后端解密后计算密码字符串的`MD5/MD6`存入数据库；也可以 `PlanB`-直接前端使用一种稳定算法加密成唯一值、后端直接将加密结果进行`MD5/MD6`，全程密码明文不出现在程序中。

#### 事件模型
事件传播指的是发生事件时传播的过程。一共按顺序分为以下三个阶段。<br>
捕获阶段：从`window`对象传导到目标节点（从上到下）的过程,直到目标的元素,为截获事件提供机会<br>
目标阶段：在当前目标触发的过程 ,目标接受事件<br>
冒泡阶段：从目标节点传导回到`window`对象（从下到上）的过程,在这个阶段对事件做出响应。<br>

执行顺序： 捕获 -> 冒泡
#### 说下浏览器的缓存机制
浏览器缓存可以分为**强缓存**和**协商缓存**，服务端可以在响应头中增加`Cache-Control/Expires`来为当前资源设置缓存有效期，`Cache-Control`的`Max-age`的优先级大于`Expires`,浏览器再次发起请求时，先判断缓存是否过期，如果没有过期则命中强缓存，直接使用浏览器本地缓存资源；如果已过期则使用协商缓存，协商缓存大致分为两种：<br>
1、唯一标识：`Etag`(服务端响应携带)和`If-None-Match`(客户端请求携带);<br>
2、最后修改时间：`Last-Modified`(服务端响应携带) & `If-Modified-Since` (客户端请求携带) ，其优先级低于`Etag`。<br>
服务端判断值是否一致，如果一致，则直接返回304通知浏览器使用本地缓存，如果不一致则返回新的资源。
#### 跨域解决标准`CORS`(跨域资源共享)
`CORS`需要浏览器和服务器同时支持，才可以实现跨域请求，目前几乎所有浏览器都支持`CORS`，`IE`则不能低于`IE10`。`CORS`的整个过程都由浏览器自动完成，前端无需做任何设置，跟平时发送`ajax`请求并无差异。`so`，实现`CORS`的关键在于服务器，只要服务器实现`CORS`接口，就可以实现跨域通信。
```js
var express = require('express');
var app = express();
var allowCrossDomain = function (req, res, next) {
  res.header('Access-Control-Allow-Origin', 'http://localhost:3001');
  res.header('Access-Control-Allow-Methods', 'GET,PUT,POST,DELETE');
  res.header('Access-Control-Allow-Headers', 'Content-Type');
  next();
}
app.use(allowCrossDomain);
```
`CORS`字段解释：<br>
`Access-Control-Allow-Methods`: 该字段必需，它的值是逗号分隔的一个字符串，表明服务器支持的所有跨域请求的方法。<br>
`Access-Control-Allow-Headers`: 如果浏览器请求包括`Access-Control-Request-Headers`字段，则`Access-Control-Allow-Headers`字段是必需的。它也是一个逗号分隔的字符串，表明服务器支持的所有头信息字段，不限于浏览器在"预检"中请求的字段。<br>
`Access-Control-Allow-Credentials`: 该字段与简单请求时的含义相同。<br>
`Access-Control-Max-Age`: 该字段可选，用来指定本次预检请求的有效期，单位为秒。
使用`CORS`简单请求，非常容易，对于前端来说无需做任何配置，与发送普通`ajax`请求无异。唯一需要注意的是，需要携带`cookie`信息时，需要将`withCredentials`设置为`true`即可


#### 微信小程序
小程序底层还是基于Webview来实现的。并没有发明创造新技术，整个框架体系。比較清晰和简单，基于Web规范，保证现有技能价值的最大化，仅仅需了解框架规范即可使用已有Web技术进行开发。易于理解和开发。<br>
MSSM：对逻辑和UI进行了全然隔离，这个跟当前流行的react，agular，vue有本质的差别，小程序逻辑和UI全然执行在2个独立的Webview里面，而后面这几个框架还是执行在一个webview里面的，假设你想。还是能够直接操作dom对象，进行ui渲染的。<br>
组件机制：引入组件化机制，可是不全然基于组件开发。跟vue一样大部分UI还是模板化渲染，引入组件机制能更好的规范开发模式，也更方便升级和维护。<br>
多种克制：不能同一时候打开超过5个窗体。打包文件不能大于1M，dom对象不能大于16000个等。这些都是为了保证更好的体验。<br>

#### 微信小程序生命周期
应用生命周期：<br>
1、onLaunch: 初始化小程序时触发，全局只触发一次<br>
2、onShow: 小程序初始化完成或用户从后台切换到前台显示时触发<br>
3、onHide: 用户从前台切换到后台隐藏时触发<br>
4、onError: 小程序发生脚本错误，或者 api 调用失败时，会触发 onError 并带上错误信息<br>

页面生命周期<br>
1、onLoad：首次进入页面加载时触发，可以在 onLoad 的参数中获取打开当前页面路径中的参数。
2、onShow：加载完成后、后台切到前台或重新进入页面时触发
3、onReady：页面首次渲染完成时触发
4、onHide：从前台切到后台或进入其他页面触发
5、onUnload：页面卸载时触发
