#### 页面性能监测？
**理解 Navigation Timing API 的性能指标**<br>
为了帮助开发者更好地衡量和改进前端页面性能，`W3C` 性能小组引入了 `Navigation Timing API`，实现了自动、精准的页面性能打点；开发者可以通过 `window.performance` 属性获取。<br>
`performance.timing` 接口（定义了从 `navigationStart` 至 `loadEventEnd` 的 21 个只读属性）

`performance.navigation`（定义了当前文档的导航信息，比如是重载还是向前向后等）

<img width="700 src='/Frontend-Interview-Question/timing.jpg' />

<img width="700 src='/Frontend-Interview-Question/timing2.jpg' />

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

