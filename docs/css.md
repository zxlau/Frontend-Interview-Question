# CSS相关
#### `link`和`@import`的区别
两者都是外部引用 `CSS` 的方式，但是存在一定的区别：<br/>
1、`link`是`XHTML`标签，除了能够加载`CSS`，还可以定义`RSS`等其他事务；而`@import`属于`CSS`范畴，只可以加载`CSS`。<br/>
2、`link`引用`CSS`时，在页面载入时同时加载；`@import`需要页面完全载入以后再加载。<br/>
3、`link`是`XHTML`标签，无兼容问题；`@import`则是在`CSS2.1`提出的，低版本的浏览器不支持。<br/>
4、`link`支持使用`Javascript`控制`DOM`改变样式；而`@import`不支持
#### `CSS`选择符有哪些？哪些属性可以继承？
```css
*   1.id选择器（ #myid）
    2.类选择器（.myclassname）
    3.标签选择器（div, h1, p）
    4.相邻选择器（h1 + p）
    5.子选择器（ul > li）
    6.后代选择器（li a）
    7.通配符选择器（ * ）
    8.属性选择器（a[rel = "external"]）
    9.伪类选择器（a:hover, li:nth-child）

*   可继承的样式： font-size font-family color, UL LI DL DD DT;

*   不可继承的样式：border padding margin width height ;
```
#### `CSS`优先级算法如何计算？
优先级就近原则，同权重情况下样式定义最近者为准;<br/>
载入样式以最后载入的定位为准;<br/>
同权重: <br/>
内联样式表（标签内部）> 嵌入样式表（当前文件中）> 外部样式表（外部文件中）<br/>
```css
!important >  id > class > tag
// important 比 内联优先级高
```




