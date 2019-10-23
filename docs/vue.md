#### 写 `React/Vue` 项目时为什么要在组件中写 `key`，其作用是什么？
`key` 的作用是为了在 `diff` 算法执行时更快的找到对应的节点，提高 `diff` 速度。<br/>
`vue` 和 `react` 都是采用 `diff` 算法来对比新旧虚拟节点，从而更新节点。在 `vue` 的 `diff` 函数中。可以先了解一下 `diff` 算法。<br/>
在交叉对比的时候，当新节点跟旧节点头尾交叉对比没有结果的时候，会根据新节点的 `key` 去对比旧节点数组中的 `key`，从而找到相应旧节点（这里对应的是一个 `key => index` 的 `map` 映射）。如果没找到就认为是一个新增节点。而如果没有 `key`，那么就会采用一种遍历查找的方式去找到对应的旧节点。一种一个 `map` 映射，另一种是遍历查找。相比而言。`map` 映射的速度更快。
#### 对于`MVVM`的理解
`MVVM` 是 `Model-View-ViewModel` 的缩写<br>
`Model`: 代表数据模型，也可以在`Model`中定义数据修改和操作的业务逻辑。我们可以把`Model`称为数据层，因为它仅仅关注数据本身，不关心任何行为<br/>`
`View`: 用户操作界面。当`ViewModel`对`Model`进行更新的时候，会通过数据绑定更新到`View`<br>
`ViewModel`： 业务逻辑层，`View`需要什么数据，`ViewModel`要提供这个数据；`View`有某些操作，`ViewModel`就要响应这些操作，所以可以说它是`Model for View`<br/>
总结： `MVVM`模式简化了界面与业务的依赖，解决了数据频繁更新。`MVVM`在使用当中，利用双向绑定技术，使得`Model`变化时，`ViewModel`会自动更新，而`ViewModel`变化时，`View`也会自动变化。
#### 请详细说下你对`vue`生命周期的理解
`vue`生命周期总共分为`8`个阶段: `beforeCreate/created`、`beforeMount/mounted`、`beforeUpdate/updated`、`beforeDestroy/destroyed`。<br>

`beforeCreate`（创建前）`vue`实例的挂载元素`$el`和数据对象`data`都是`undefined`, 还未初始化<br>

`created` (创建后) 完成了`data`数据初始化, `$el`还未初始化

`beforeMount` (载入前) `vue`实例的`$el`和`data`都初始化了, 相关的`render`函数首次被调用。实例已完成以下的配置：编译模板，把`data`里面的数据和模板生成`html`。注意此时还没有挂载`html`到页面上。

`mounted` (载入后) 在`el` 被新创建的`vm.$el`替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的`html`内容替换`el`属性指向的`DOM`对象。完成模板中的`html`渲染到`html`页面中。此过程中进行`ajax`交互

`beforeUpdate` (更新前) 在数据更新之前调用，发生在虚拟`DOM`重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。

`updated` （更新后） 在由于数据更改导致的虚拟`DOM`重新渲染和打补丁之后调用。调用时，组件`DOM`已经更新，所以可以执行依赖于`DOM`的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。

`beforeDestroy` (销毁前） 在实例销毁之前调用。实例仍然完全可用。

`destroyed` (销毁后） 在实例销毁之后













