# CSS相关
#### `BFC(Block Formatting Context))`块级格式化上下文
`BFC`有以下特性：<br/>
1、内部的Box会在垂直方向，从顶部开始一个接一个地放置。<br/>
2、Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生叠加。<br/>
3、每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。<br/>
4、BFC的区域不会与float box叠加。<br/>
5、BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之亦然。<br/>
6、计算BFC的高度时，浮动元素也参与计算。<br/>

怎样触发`BFC`:<br/>
1、`float`除了`none`之外的属性。
2、`overflow`除了`visible`之外的属性`(auto, hidden, scroll)`。
3、`display` `(table-cell，table-caption，inline-block, flex, inline-flex)`
4、`position` `(abslute、fixed)`
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
#### `CSS3`新增伪类有那些？
```css
p:first-of-type /*选择属于其父元素的首个 <p> 元素的每个 <p> 元素。*/
p:last-of-type  /*选择属于其父元素的最后 <p> 元素的每个 <p> 元素。*/
p:only-of-type  /*选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。*/
p:only-child    /*选择属于其父元素的唯一子元素的每个 <p> 元素。*/
p:nth-child(2)  /*选择属于其父元素的第二个子元素的每个 <p> 元素。*/

:after          /*在元素之前添加内容,也可以用来做清除浮动。*/
:before         /*在元素之后添加内容*/
:enabled        
:disabled       /*控制表单控件的禁用状态。*/
:checked        /*单选框或复选框被选中。*/
```
#### 如何居中`div`？
1、水平居中：给`div`设置一个宽度，然后添加`margin:0 auto`属性
```css
div{
  width:200px;
  margin:0 auto;
 }
```
2、水平垂直居中一
```css
div {
  position: relative;     /* 相对定位或绝对定位均可 */
  width:500px; 
  height:300px;
  top: 50%;
  left: 50%;
  margin: -150px 0 0 -250px;      /* 外边距为自身宽高的一半 */
  background-color: pink;     /* 方便看效果 */
 }
```
3、水平垂直居中二<br/>
未知容器宽高，利用`transform`属性
```css
div {
  position: absolute;     /* 相对定位或绝对定位均可 */
  width:500px; 
  height:300px;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  background-color: pink;     /* 方便看效果 */
}
```
4、水平垂直居中三
```css
.container {
  display: flex; 
  align-items: center;        /* 垂直居中 */
  justify-content: center;    /* 水平居中 */
}
.container div {
  width: 100px;
  height: 100px;
  background-color: pink;     /* 方便看效果 */
}
```
#### `CSS3`有哪些新特性？
新增各种`CSS`选择器  `:not(.input)`：所有 `class` 不是`input`的节点）<br/>
圆角           `border-radius:8px`<br/>
多列布局       `multi-column`<br/>
阴影和反射     `Shadow\Reflect`<br/>
文字特效       `text-shadow`<br/>
文字渲染       `Text-decoration`<br/>
线性渐变       `gradient`<br/>
旋转          `transform`<br/>
缩放,定位,倾斜,动画,多背景<br/>
#### 为什么要初始化`CSS`样式？
因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对`CSS`初始化往往会出现浏览器之间的页面显示差异。

当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。
#### 盒子模型
盒子模型包括`content+padding+border+margin`<br/>
标准盒子模型：宽度=内容的宽度（`content`）即默认模型：`content-box`<br/>
低版本IE盒子模型：宽度=`content+border+padding`即`border-box`<br/>
一般都设置为`border-box`<br/>
#### 弹性盒模型`flex`
`CSS3`弹性盒（`Flexible Box` 或 `flexbox`），是一种当页面需要适应不同的屏幕大小以及设备类型时确保元素拥有恰当的行为的布局方式。设为 `Flex` 布局以后，子元素的`float、clear`和`vertical-align`属性将失效。<br/>
弹性盒子由弹性容器`Flex container`和弹性子元素`Flex item`组成。<br/>
弹性容器通过设置`display`属性的值为`flex`或`inline-flex`将其定义为弹性容器。<br/>

属性 | 描述 | 取值
-|-
`flex-direction` | 指定弹性容器中子元素排列方式 | `row`<br/>`row-reverse`<br/>`column`<br/>`column-reverse`
`flex-wrap` | 设置弹性盒子的子元素超出父容器时是否换行 | `nowrap`<br/>`wrap`<br/>`wrap-reverse`
`flex-flow` | `flex-direction`属性和`flex-wrap`属性的简写形式 | 默认为`row nowrap`
`justify-content` | 设置弹性盒子元素在主轴（横轴）方向上的对齐方式 | `flex-start`<br/>`flex-end`<br/>`center`<br/>`space-between`<br/>`space-around`
`align-items` | 设置弹性盒子元素在侧轴（纵轴）方向上的对齐方式 | `flex-start`<br/>`flex-end`<br/>`center`<br/>`baseline `<br/>`stretch`
`align-content` | 属性定义了多根轴线的对齐方式 | `flex-start`<br/>`flex-end`<br/>`center`<br/>`space-between`<br/>`space-around`<br/>`stretch`

设置在项目`item`上的属性

属性 | 描述 | 取值
-|-
`order` | 属性定义项目的排列顺序,数值越小，排列越靠前 | 默认为`0`
`flex-grow` | 定义项目的放大比例 | 默认为`0`<br/>(存在剩余空间，也不放大)
`flex-shrink` | 定义了项目的缩小比例 | 默认为`1`<br/>(如果空间不足，该项目将缩小)
`flex-basis` | 定义了在分配多余空间之前，项目占据的主轴空间 | 默认值是`auto`
`flex` | `flex-grow`、`flex-shrink`和`flex-basis`的简写 | 默认值`0 1 auto`<br/>后两个属性可选
`align-self` | 允许单个项目有与其他项目不一样的对齐方式,align-items | 默认值`auto`
#### `display:none`与`visibility:hidden`
很多前端的同学认为`visibility: hidden`和`display: none`的区别仅仅在于`display: none`隐藏后的元素不占据任何空间，而`visibility: hidden`隐藏后的元素空间依旧保留<br/>
实际上没那么简单，`visibility`是一个非常有故事性的属性<br/>
`visibility`具有继承性，给父元素设置`visibility:hidden`;子元素也会继承这个属性。<br/>
但是如果重新给子元素设置`visibility:visible`,则子元素又会显示出来。这个和`display: none`有着质的区别<br/>
#### 清除浮动
```css
.clear-fix:after {
  display:block;
  clear:both;
  content:"";
  visibility:hidden;
  height:0
}
.clear-fix:after {
  content: '';
  display: table;
  clear: both;
}
```
#### `css`三角形
```css
.triangle-up {
  width: 0;
  height: 0;
  border-bottom: 100px solid #000;
  border-left: 50px solid #000;
  border-right: 50px solid #000;
}
.triangle-down {
  width: 0;
  height: 0;
  border-top: 100px solid #000;
  border-left: 50px solid #000;
  border-right: 50px solid #000;
}
```
#### `rgba`和`opacity`的透明有何不同？
`opacity`会继承父元素的`opacity`属性，而`RGBA`设置的元素的后代元素不会继承不透明属性。简单来说就是`opacity`作用于元素和元素所有内容的透明。
#### 怎么让浏览器支持小于`12px`的文字
```css
tranform: scale()
```
#### 尽可能多的方法隐藏一个`html`元素
```css
opacity: 0;
display: none;
visibility: hidden;

width: 0;
height: 0;

<input type="hidden" name="看不见我">
```
#### `less`等预处理器好处
文件切分 模块化 选择符嵌套 变量 运算 函数 `Mixin` 工程化<br/>
能够更快速的编写代码 结构清晰，便于维护
#### 什么要浮动，什么时候需要清除浮动，清除浮动都有哪些方法
使用浮动之后，父元素的高度会塌陷，变成0，导致布局有问题，这个时候需要清除浮动。
```css
.clearfix:after {
  content: '';
  display: bolck;
  height: 0;
  visibility: hidden;
  clear: both
}
.clearfix:after {
  content: '';
  display: table;
  clear: both
}
```
#### 分析比较 `opacity: 0、visibility: hidden、display: none`优劣和适用场景
`display: none` (不占空间，不能点击)（场景，显示出原来这里不存在的结构）<br>
`visibility: hidden`（占据空间，不能点击）（场景：显示不会导致页面结构发生变动，不会撑开）<br>
`opacity: 0`（占据空间，可以点击）（场景：可以跟`transition`搭配）
#### 已知如下代码，如何修改才能让图片宽度为 `300px` ？注意下面代码不可修改。
```html
<img src="1.jpg" style="width:480px!important;”>
```
```html
<img src="1.jpg" style="width:480px!important; max-width: 300px">
<img src="1.jpg" style="width:480px!important; transform: scale(0.625, 1);" >
<img src="1.jpg" style="width:480px!important; width:300px!important;">
```
#### 如何解决移动端 `Retina` 屏 `1px` 像素问题
```css
viewport + rem
background-image
scale
border-image
box-shadow
```







