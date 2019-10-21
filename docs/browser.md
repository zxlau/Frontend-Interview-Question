# 浏览器相关
#### 浏览器如何渲染页面
1、浏览器通过`HTML Parser`根据深度遍历的原则把HTML解析成`DOM Tree`。<br/>
2、将`CSS`解析成 `CSS Rule Tree (CSSOM Tree)`。<br/>
3、根据`DOM`树和`CSSOM`树来构造`render Tree`。<br/>
4、`layout`: 根据得到的`render Tree`来计算所有节点的位置。<br/>
5、`paint`: 遍历`render` 树，并调用硬件图形`API`来绘制每个节点。
#### 介绍一下你对浏览器内核的理解？
主要分成两部分： 渲染引擎和`js`引擎<br/>
渲染引擎：负责取得网页的内容（`HTML、XML`、图像等等）、整理讯息（例如加入`CSS`等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。<br/>
`JS`引擎：解析和执行`javascript`来实现网页的动态效果。<br/>

常见内核<br/>
`Trident`内核：`IE,MaxThon,TT,The World,360`,搜狗浏览器等。<br/>
`Gecko`内核：`Netscape6`及以上版本，`FF,MozillaSuite/SeaMonkey`等。<br/>
`Presto`内核：`Opera7`及以上。<br/>
`Webkit`内核：`Safari,Chrome`等。
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


