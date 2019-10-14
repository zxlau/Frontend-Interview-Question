# 浏览器相关
#### 浏览器如何渲染页面
1、浏览器通过`HTML Parser`根据深度遍历的原则把HTML解析成`DOM Tree`。<br/>
2、将`CSS`解析成 `CSS Rule Tree (CSSOM Tree)`。<br/>
3、根据`DOM`树和`CSSOM`树来构造`render Tree`。<br/>
4、`layout`: 根据得到的`render Tree`来计算所有节点的位置。<br/>
5、`paint`: 遍历`render` 树，并调用硬件图形`API`来绘制每个节点。