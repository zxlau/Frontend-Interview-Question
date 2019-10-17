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










