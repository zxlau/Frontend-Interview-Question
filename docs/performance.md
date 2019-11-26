#### 页面性能监测？
**理解 Navigation Timing API 的性能指标**<br>
为了帮助开发者更好地衡量和改进前端页面性能，`W3C` 性能小组引入了 `Navigation Timing API`，实现了自动、精准的页面性能打点；开发者可以通过 `window.performance` 属性获取。<br>
`performance.timing` 接口（定义了从 `navigationStart` 至 `loadEventEnd` 的 21 个只读属性）

`performance.navigation`（定义了当前文档的导航信息，比如是重载还是向前向后等）

<img width="700" src='/Frontend-Interview-Question/images/timing.jpg' />

<img width="700" src='/Frontend-Interview-Question/images/timing2.jpg' />

**确定统计起始点 （ `navigationStart vs fetchStart` ）**<br>
页面性能统计的起始点时间，应该是用户输入网址回车后开始等待的时间。一个是通过`navigationStart`获取，相当于在 `URL` 输入栏回车或者页面按 `F5` 刷新的时间点；另外一个是通过 `fetchStart`，相当于浏览器准备好使用 `HTTP` 请求获取文档的时间。<br>
从开发者实际分析使用的场景，浏览器重定向、卸载页面的耗时对页面加载分析并无太大作用；通常建议使用 `fetchStart` 作为统计起始点。<br>

**首字节**<br>
主文档返回第一个字节的时间，是页面加载性能比较重要的指标。对用户来说一般无感知，对于开发者来说，则代表访问网络后端的整体响应耗时。

**白屏时间**<br>
用户看到页面展示出现一个元素的时间。很多人认为白屏时间是页面返回的首字节时间，但这样其实并不精确，因为头部资源还没加载完毕，页面也是白屏。<br>
相对来说具备「白屏时间」统计意义的指标，可以取 `domLoading - fetchStart`，此时页面开始解析 `DOM`树，页面渲染的第一个元素也会很快出现。<br>
从 `W3C Navigation Timing Level 2` 的方案设计，可以直接采用 `domInteractive - fetchStart` ，此时页面资源加载完成，即将进入渲染环节。

**首屏时间**<br>
首屏时间是指页面第一屏所有资源完整展示的时间。这是一个对用户来说非常直接的体验指标，但是对于前端却是一个非常难以r统计衡量的指标。<br>
具备一定意义上的指标可以使用，`domContentLoadedEventEnd - fetchStart`，甚至使用`loadEventStart - fetchStart`，此时页面 `DOM` 树已经解析完成并且显示内容。<br>
统计页面性能指标的方法：<br>
```js
let times = {};
let t = window.performance.timing;

// 优先使用 navigation v2  https://www.w3.org/TR/navigation-timing-2/
if (typeof win.PerformanceNavigationTiming === 'function') {
  try {
    var nt2Timing = performance.getEntriesByType('navigation')[0]
    if (nt2Timing) {
      t = nt2Timing
    }
  } catch (err) {
  }
}
//重定向时间
times.redirectTime = t.redirectEnd - t.redirectStart;
//dns 查询耗时
times.dnsTime = t.domainLookupEnd - t.domainLookupStart;
//TTFB 读取页面第一个字节的时间
times.ttfbTime = t.responseStart - t.navigationStart;
//DNS 缓存时间
times.appcacheTime = t.domainLookupStart - t.fetchStart;
//卸载页面的时间
times.unloadTime = t.unloadEventEnd - t.unloadEventStart;
//tcp 连接耗时
times.tcpTime = t.connectEnd - t.connectStart;
//request 请求耗时
times.reqTime = t.responseEnd - t.responseStart;
//解析 dom 树耗时
times.analysisTime = t.domComplete - t.domInteractive;
//白屏时间 
times.blankTime = (t.domInteractive || t.domLoading) - t.fetchStart;
//domReadyTime
times.domReadyTime = t.domContentLoadedEventEnd - t.fetchStart;
```

**SPA 盛行之际**<br>

注意点: 通过 `window.performance.timing` 所获的的页面渲染所相关的数据，在 `SPA` 应用中改变了 `url` 但不刷新页面的情况下是不会更新的。因此仅仅通过该 `api` 是无法获得每一个子路由所对应的页面渲染的时间。如果需要上报切换路由情况下每一个子页面重新 `render` 的时间，需要自定义上报。

数据上报方式: 测量好时间后，就需要将数据发送给服务端。页面性能统计数据对丢失率要求比较低，且性能统计应该在尽量不影响主流程的逻辑和页面性能的前提下进行。

`navigator.sendBeacon`: 大部分现代浏览器都支持 `navigator.sendBeacon `方法。这个方法可以用来发送一些统计和诊断的小量数据，特别适合上报统计的场景。
```js
window.addEventListener('unload', logData, false);

function logData() {
    navigator.sendBeacon("/log", analyticsData);
}
```

#### `js`生产环境错误监测
参考：`https://www.cnblogs.com/warm-stranger/p/9417084.html`<br>

监控流程：监控错误 -> 搜集错误 -> 存储错误 -> 分析错误 -> 错误报警-> 定位错误 -> 解决错误<br>
首先，我们应该对`Js`报错情况有个大致的了解，这样才能够及时的了解前端项目的健康状况。所以我们需要分析出一些必要的数据。<br>
如：一段时间内，应用`JS`报错的走势(`chart`图表)、`JS`错误发生率、`JS`错误在`PC`端发生的概率、`JS`错误在`IOS`端发生的概率、`JS`错误在`Android`端发生的概率，以及`JS`错误的归类。<br>
然后，我们再去其中的`Js`错误进行详细的分析，辅助我们排查出错的位置和发生错误的原因。<br>
如：`JS`错误类型、 `JS`错误信息、`JS`错误堆栈、`JS`错误发生的位置以及相关位置的代码；`JS`错误发生的几率、浏览器的类型，版本号，设备机型等等辅助信息。<br>

为了得到这些数据，我们需要在上传的时候将其分析出来。在众多日志分析中，很多字段及功能是重复通用的，所以应该将其封装起来。
```js
// 设置日志对象类的通用属性
function setCommonProperty() {
  this.happenTime = new Date().getTime(); // 日志发生时间
  this.webMonitorId = WEB_MONITOR_ID;     // 用于区分应用的唯一标识（一个项目对应一个）
  this.simpleUrl =  window.location.href.split('?')[0].replace('#', ''); // 页面的url
  this.customerKey = utils.getCustomerKey(); // 用于区分用户，所对应唯一的标识，清理本地数据后失效
  this.pageKey = utils.getPageKey();  // 用于区分页面，所对应唯一的标识，每个新页面对应一个值
  this.deviceName = DEVICE_INFO.deviceName;
  this.os = DEVICE_INFO.os + (DEVICE_INFO.osVersion ? " " + DEVICE_INFO.osVersion : "");
  this.browserName = DEVICE_INFO.browserName;
  this.browserVersion = DEVICE_INFO.browserVersion;
  // TODO 位置信息, 待处理
  this.monitorIp = "";  // 用户的IP地址
  this.country = "china";  // 用户所在国家
  this.province = "";  // 用户所在省份
  this.city = "";  // 用户所在城市
  // 用户自定义信息， 由开发者主动传入， 便于对线上进行准确定位
  this.userId = USER_INFO.userId;
  this.firstUserParam = USER_INFO.firstUserParam;
  this.secondUserParam = USER_INFO.secondUserParam;
}

// JS错误日志，继承于日志基类MonitorBaseInfo
function JavaScriptErrorInfo(uploadType, errorMsg, errorStack) {
  setCommonProperty.apply(this);
  this.uploadType = uploadType;
  this.errorMessage = encodeURIComponent(errorMsg);
  this.errorStack = errorStack;
  this.browserInfo = BROWSER_INFO;
}
JavaScriptErrorInfo.prototype = new MonitorBaseInfo();
```
封装了一个`Js`错误对象`JavaScriptErrorInfo`，用以保存页面中产生的`Js`错误。其中，`setCommonProperty`用以设置所有日志对象的通用属性。<br>
1）重写`window.onerror` 方法， 大家熟知，监控JS错误必然离不开它，有人对他进行了测试测试介绍感觉也是比较用心了<br>
2）重写`console.error`方法，为什么要重写这个方法，我不能够给出明确的答案，如果`App`首次向浏览器注入的`Js`代码报错了，`window.onerror`是无法监控到的，所以只能重写`console.error`的方式来进行捕获，也许会有更好的办法。待`window.onerror`成功后，此方法便不再需要用了<br>
3）重写`window.onunhandledrejection`方法。 当你用到`Promise`的时候，而你又忘记写`reject`的捕获方法的时候，系统总是会抛出一个叫 `Unhandled Promise rejection`. 没有堆栈，没有其他信息，特别是在写`fetch`请求的时候很容易发生。 所以我们需要重写这个方法，以帮助我们监控此类错误<br>
下边是启动`JS`错误监控代码:
```js
/**
 * 页面JS错误监控
 */
function recordJavaScriptError() {
  // 重写console.error, 可以捕获更全面的报错信息
  var oldError = console.error;
  console.error = function () {
    // arguments的长度为2时，才是error上报的时机
    // if (arguments.length < 2) return;
    var errorMsg = arguments[0] && arguments[0].message;
    var url = WEB_LOCATION;
    var lineNumber = 0;
    var columnNumber = 0;
    var errorObj = arguments[0] && arguments[0].stack;
    if (!errorObj) errorObj = arguments[0];
    // 如果onerror重写成功，就无需在这里进行上报了
    !jsMonitorStarted && siftAndMakeUpMessage(errorMsg, url, lineNumber, columnNumber, errorObj);
    return oldError.apply(console, arguments);
  };
  // 重写 onerror 进行jsError的监听
  window.onerror = function(errorMsg, url, lineNumber, columnNumber, errorObj)
  {
    jsMonitorStarted = true;
    var errorStack = errorObj ? errorObj.stack : null;
    siftAndMakeUpMessage(errorMsg, url, lineNumber, columnNumber, errorStack);
  };

  function siftAndMakeUpMessage(origin_errorMsg, origin_url, origin_lineNumber, origin_columnNumber, origin_errorObj) {
    var errorMsg = origin_errorMsg ? origin_errorMsg : '';
    var errorObj = origin_errorObj ? origin_errorObj : '';
    var errorType = "";
    if (errorMsg) {
      var errorStackStr = JSON.stringify(errorObj)
      errorType = errorStackStr.split(": ")[0].replace('"', "");
    }
    var javaScriptErrorInfo = new JavaScriptErrorInfo(JS_ERROR, errorType + ": " + errorMsg, errorObj);
    javaScriptErrorInfo.handleLogInfo(JS_ERROR, javaScriptErrorInfo);
  };
};
```
#### 公司前端项目部署
**正式环境：**<br>
公网转内网`（CLB -> LB）`---> `www.XXXXX.com`<br>
如果路由中有带`/api/` ---> 则`LB`会转发至后端服务地址，比如`c.restgetway`<br>
如果路由没有带`/api/` ---> 则`LB`会转发至前端服务地址，比如`c.fe`<br>
然后前端部署`c.fe`的时候，`html`文件是和后端服务部署在同一个服务上，都是`www.XXXXX.com`。所以不存在跨域的问题。<br>
`webpack`打包之后的`css、js`文件都会部署到`cdn`上面，然后`html`引用`cdn`的地址。<br>

**开发环境：**<br>
基本和正式环境一致，但是少了一层公网转内网。然后`LB`层使用`nginx`代替，使用`nginx``来做转发。

**本地环境：**<br>
`webpack`配置 `umock` 地址，`umock` 使用`node`进行转发。











