# HTML相关
#### `Doctype`作用？严格模式与混杂模式如何区分？它们有何意义?
- `<!DOCTYPE>`声明位于位于`HTML`文档中的第一行，处于 `<html>` 标签之前。告知浏览器的解析器用什么文档标准解析这个文档。
- 严格模式的排版和 JS 运作模式是 以该浏览器支持的最高标准运行
- 在混杂模式中，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止站点无法工作DOCTYPE 不存在或格式不正确会导致文档以混杂模式呈现
####  简述一下你对 `HTML` 语义化的理解？
用正确的标签做正确的事情。<br>
`html` 语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析；即使在没有样式 `CSS` 情况下也以一种文档格式显示，并且是容易阅读的;<br>
搜索引擎的爬虫也依赖于 `HTML` 标记来确定上下文和各个关键字的权重，利于`SEO`;<br>
使阅读源代码的人对网站更容易将网站分块，便于阅读维护理解。<br>
去掉或者丢失样式的时候能够让页面呈现出清晰的结构。<br>
方便其他设备解析(如屏幕阅读器、盲人阅读器、移动设备)以意义的方式来渲染网页。<br>

#### meta标签
`meta`是`HTML`语言`head`区的一个辅助性标签，位于文档的头部，不包含任何内容。 标签的属性定义了与文档相关联的名称/值对。<br>
`meta`元素可提供相关页面的元信息`（meta-information）`，比如针对搜索引擎和更新频度的描述和关键词。<br>
`meta` 元素往往不会引起用户的注意，但是`meta`对整个网页有影响，会对网页能否被搜索引擎检索，和在搜索中的排名起着关键性的作用。<br>

`meta`有个必须的属性`content`用于表示需要设置的项的值。<br>
`meta`存在两个非必须的属性`http-equiv`和`name`, 用于表示要设置的项。<br>
比如`<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">`,设置的项是`Content-Security-Policy`设置的值是`upgrade-insecure-requests`。<br>

| 属性 | 值 | 描述 |
|--|
|charset|character_set|定义文档的字符编码。|
|content|text|定义与 http-equiv 或 name 属性相关的元信息。|
|http-equiv|content-typ<br>edefault-style<br>refresh|把 content 属性关联到 HTTP 头部。|
|name|application-nam<br>eauthor<br>description<br>generator<br>keywords|把 content 属性关联到一个名称。|

#### 说说 title 和 alt 属性
- 两个属性都是当鼠标滑动到元素上的时候显示
- `alt` 是 `img` 的特有属性，是图片内容的等价描述，图片无法正常显示时候的替代文字。
- `title` 属性可以用在除了`base`，`basefont`，`head`，`html`，`meta`，`param`，`script`和`title`之外的所有标签，是对`dom`元素的一种类似注释说明。
#### html5 有哪些新特性、移除了哪些元素
- 新增元素：
绘画 `canvas`<br/>
用于媒介回放的 `video` 和 `audio` 元素<br/>
本地离线存储 `localStorage` 长期存储数据，浏览器关闭后数据不丢失<br/>
`sessionStorage` 的数据在浏览器关闭后自动删除<br/>
语义化更好的内容元素，比如 `article` `、footer、header、nav、section`<br/>
表单控件 ，`calendar` 、 `date` 、 `time` 、 `email` 、 `url` 、 `search`<br/>
新的技术 `webworker` 、 `websocket` 、 `Geolocation`<br/>

移除的元素：<br/>
纯表现的元素： `basefont` 、 `big` 、 `center` 、 `font` 、 `s` 、 `strike` 、 `tt` 、 `u`<br/>
对可用性产生负面影响的元素： `frame` 、 `frameset` 、 `noframes`<br/>


#### `href`与`src`
`href (Hypertext Reference)`指定网络资源的位置，从而在当前元素或者当前文档和由当前属性定义的需要的锚点或资源之间定义一个链接或者关系。（目的不是为了引用资源，而是为了建立联系，让当前标签能够链接到目标地址）。<br/>
`src source`，指向外部资源的位置，指向的内容将会应用到文档中当前标签所在位置。<br/>
`href`与`src`的区别:<br/>
1、请求资源类型不同：`href` 指向网络资源所在位置，建立和当前元素（锚点）或当前文档（链接）之间的联系。在请求 `src` 资源时会将其指向的资源下载并应用到文档中，比如 `JavaScript` 脚本，`img`图片；<br/>
2、作用结果不同：`href` 用于在当前文档和引用资源之间确立联系；`src` 用于替换当前内容；<br/>
3、浏览器解析方式不同：当浏览器解析到`src` ，会暂停其他资源的下载和处理，直到将该资源加载、编译、执行完毕，图片和框架等也如此，类似于将所指向资源应用到当前内容。这也是为什么建议把 `js` 脚本放在底部而不是头部的原因。
#### 行内元素有哪些？块级元素有哪些？
行内元素有：`a b span img input select strong` <br/>
块级元素有：`div ul ol li dl dt dd h1 h2 h3 h4…p`
#### `iframe`有那些缺点？
`iframe`会阻塞主页面的`Onload`事件；搜索引擎的检索程序无法解读这种页面，不利于`SEO`; `iframe`和主页面共享连接池，而浏览器对相同域名的连接有限制，所以会影响页面的并行加载。<br/>

使用`iframe`之前需要考虑这两个缺点。如果需要使用`iframe`，最好是通过`javascript`动态给`iframe`添加`src`属性值，这样可以绕开以上两个问题。

#### HTML W3C的标准
标签闭合、标签小写、不乱嵌套、使用外链 css 和 js 、结构行为表现的分离

#### viewport的content属性作用
```html
<meta name="viewport" content="" />
    width viewport的宽度[device-width | pixel_value]width如果直接设置pixel_value数值，大部分的安卓手机不支持，但是ios支持；
    height – viewport 的高度 （范围从 223 到 10,000 ）
    user-scalable [yes | no]是否允许缩放
    initial-scale [数值] 初始化比例（范围从 > 0 到 10）
    minimum-scale [数值] 允许缩放的最小比例
    maximum-scale [数值] 允许缩放的最大比例
    target-densitydpi 值有以下（一般推荐设置中等响度密度或者低像素密度，后者设置具体的值dpi_value，另外webkit内核已不准备再支持此属性）
         -- dpi_value 一般是70-400//没英寸像素点的个数
         -- device-dpi设备默认像素密度
         -- high-dpi 高像素密度
         -- medium-dpi 中等像素密度
         -- low-dpi 低像素密度
```
#### 怎样处理 移动端 1px 被 渲染成 2px 问题?

局部处理：<br/>
`mate` 标签中的 `viewport` 属性 ， `initial-scale` 设置为 1<br/>
`rem` 按照设计稿标准走，外加利用 `transfrome` 的 `scale(0.5)` 缩小一倍即可；<br/>
全局处理：<br/>
`mate` 标签中的 `viewport` 属性 ， `initial-scale` 设置为 `0.5`<br/>
`rem` 按照设计稿标准走即可<br/>

#### meta 相关
```html
<!DOCTYPE html> <!--H5标准声明，使用 HTML5 doctype，不区分大小写-->
<head lang=”en”> <!--标准的 lang 属性写法-->
<meta charset=’utf-8′> <!--声明文档使用的字符编码-->
<meta http-equiv=”X-UA-Compatible” content=”IE=edge,chrome=1″/> <!--优先使用指定浏览器使用特定的文档模式-->
<meta name=”description” content=”不超过150个字符”/> <!--页面描述-->
<meta name=”keywords” content=””/> <!-- 页面关键词-->
<meta name=”author” content=”name, email@gmail.com”/> <!--网页作者-->
<meta name=”robots” content=”index,follow”/> <!--搜索引擎抓取-->
<meta name=”viewport” content=”initial-scale=1, maximum-scale=3 />
<meta name=”apple-mobile-web-app-title” content=”标题”> <!--iOS 设备 begin-->
<meta name=”apple-mobile-web-app-capable” content=”yes”/> <!--添加到主屏后的标
是否启用 WebApp 全屏模式，删除苹果默认的工具栏和菜单栏-->
<meta name=”apple-mobile-web-app-status-bar-style” content=”black”/>
<meta name=”renderer” content=”webkit”> <!-- 启用360浏览器的极速模式(webkit)-->
<meta http-equiv=”X-UA-Compatible” content=”IE=edge”> <!--避免IE使用兼容模式-->
<meta http-equiv=”Cache-Control” content=”no-siteapp” /> <!--不让百度转码-->
<meta name=”HandheldFriendly” content=”true”> <!--针对手持设备优化，主要是针对一些老的不识别viewport的浏览器-->
<meta name=”MobileOptimized” content=”320″> <!--微软的老式浏览器-->
<meta name=”screen-orientation” content=”portrait”> <!--uc强制竖屏-->
<meta name=”x5-orientation” content=”portrait”> <!--QQ强制竖屏-->
<meta name=”full-screen” content=”yes”> <!--UC强制全屏-->
<meta name=”x5-fullscreen” content=”true”> <!--QQ强制全屏-->
<meta name=”browsermode” content=”application”> <!--UC应用模式-->
<meta name=”x5-page-mode” content=”app”> <!-- QQ应用模式-->
<meta name=”msapplication-tap-highlight” content=”no”> <!--windows phone
设置页面不缓存-->
<meta http-equiv=”pragma” content=”no-cache”>
<meta http-equiv=”cache-control” content=”no-cache”>
<meta http-equiv=”expires” content=”0″>
```

#### 图片懒加载原理
在图片没有进入可视区的时候，不给src属性赋值，等图片进入到了可视区，再给src属性赋值，去请求图。

