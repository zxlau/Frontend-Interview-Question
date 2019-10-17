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
