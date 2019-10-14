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
#### `cookie、session、sessionStorage、localStorage`
`cookie`是网站为了标示用户身份而储存在用户本地的数据（通常经过加密）。 `cookie`数据始终在同源的`http`请求中携带（即使不需要），也会在浏览器和服务器间来回传递。<br/>
`session`和`cookie`的作用有点类似，都是为了存储用户相关的信息。服务器使用`session`把用户的信息临时保存在了服务器上，用户离开网站后`session`会被销毁。不同的是，`cookie`是存储在本地浏览器，而`session`存储在服务器。存储在服务器的数据会更加的安全，不容易被窃取。`cookie`数据大小不能超过`4k`。<br/>
一般有两种存储方式：<br/>
1、存储在服务端：通过`cookie`存储一个`session_id`，然后具体的数据则是保存在`session`中。如果用户已经登录，则服务器会在`cookie`中保存一个`session_id`，下次再次请求的时候，会把该`session_id`携带上来，服务器根据`session_id`在`session`库中获取用户的`session`数据。就能知道该用户到底是谁，以及之前保存的一些状态信息。这种专业术语叫做`server side session`。<br/>
2、将`session`数据加密，然后存储在`cookie`中。这种专业术语叫做`client side session`。`flask`采用的就是这种方式，但是也可以替换成其他形式。



