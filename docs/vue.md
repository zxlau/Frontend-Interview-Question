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
#### `Vue`的双向数据绑定原理是什么
`vue.js`是采用数据劫持结合发布者-订阅者模式的方式，通过`Object.defineProperty()`来劫持各个属性的`setter、getter`，读取数据会触发`getter`，修改数据会触发`setter`。在数据变动时发布消息给订阅者，触发相应的监听回调。

专门收集依赖的类`Dep`,它用来收集依赖、删除依赖、通过`notify`方法向依赖发送消息等

收集的依赖就是`watcher`,通过哪个`wathcher`触发了`getter`,就会把哪个`watcher`收集到`Dep`中。当数据发生变化时，会循环依赖列表，把所有的`watcher`都通知一遍。

`Data、Observe、Dep`和`Watcher`之间的关系：`Data`通过`Observe`转换成`getter/setter`的形式来追踪变化。当外界通过`watcher`读取数据时，会触发`getter`从而将`watcher`添加到依赖中。当数据发生了变化时， 会触发`setter`，从而向`Dep`中的依赖`(watcher)`发送通知。`watcher`接收到通知后，会向外界发送通知，变化通知到外界后可能触发视图更新，也有可能触发用户的某个回调函数等。
#### `Proxy`相比于`defineProperty`的优势
`defineProperty`属性主要有三个缺点:<br>
不能监听数组的变化<br>
必须遍历对象的每个属性<br>
必须深层遍历嵌套的对象<br>
`Proxy`：针对整个对象，而不是对象的某个属性，所以也就不需要对 `keys` 进行遍历。<br>
支持数组：`Proxy`不需要对数组的方法进行重载
#### `vue-router` 有哪几种导航守卫?
**vue-router全局有三个守卫***<br>
`router.beforeEach` 全局前置守卫 进入路由之前<br>
`router.beforeResolve` 全局解析守卫`(2.5.0+)` 在`beforeRouteEnter`调用之后调用<br>
`router.afterEach` 全局后置钩子 进入路由之后<br>
```js
// main.js 入口文件
import router from './router'; // 引入路由
router.beforeEach((to, from, next) => { 
  next();
});
router.beforeResolve((to, from, next) => {
  next();
});
router.afterEach((to, from) => {
  console.log('afterEach 全局后置钩子');
});
```
**路由独享守卫**<br/>
如果你不想全局配置守卫的话，你可以为某些路由单独配置守卫：
```js
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => { 
        // 参数用法什么的都一样,调用顺序在全局前置守卫后面，所以不会被全局守卫覆盖
        // ...
      }
    }
  ]
})
```
**路由组件内的守卫**
`beforeRouteEnter` 进入路由前, 在路由独享守卫后调用不能获取组件实例`this`，组件实例还没被创建<br>
`beforeRouteUpdate(2.2)` 路由复用同一个组件时, 在当前路由改变，但是该组件被复用时调用 可以访问组件实例`this`<br
`beforeRouteLeave`离开当前路由时, 导航离开该组件的对应路由时调用，可以访问组件实例`this`
#### `Vue`的路由实现：`hash`模式和`history`模式
**hash模式**<br>
在浏览器中符号`“#”`，#以及#后面的字符称之为`hash`，用`window.location.hash`读取。<br>
特点：`hash`虽然在`URL`中，但不被包括在`HTTP`请求中；用来指导浏览器动作，对服务端安全无用，`hash`不会重加载页面。 `hash`模式下，仅`hash`符号之前的内容会被包含在请求中，如 `http://www.xiaogangzai.com`，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 `404` 错误。<br>

**history模式**<br>
`history`采用`HTML5`的新特性；且提供了两个新方法：`pushState()`,`replaceState()`可以对浏览器历史记录栈进行修改，以及`popState`事件的监听到状态变更。 `history`模式下，前端的`URL`必须和实际向后端发起请求的`URL`一致，如 `http://www.xxx.com/items/id`。后端如果缺少对 `/items/id` 的路由处理，将返回 `404` 错误。`Vue-Router` 官网里如此描述：“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 `URL` 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 `app` 依赖的页面。”
#### `vuex`是什么？怎么使用？哪种功能场景使用它？
`vuex`就是一个仓库，仓库里放了很多对象。其中`state`就是数据源存放地，对应于一般`vue`对象里面的`data`<br>
`state`里面存放的数据是响应式的，`vue` 组件从 `store` 读取数据，若是 `store` 中的数据发生改变，依赖这相数据的组件也会发生更新<br/>
```js
import { mapGetters, mapActions } from 'vuex'

computed() {
  ...mapGetters({
    // getter数据
  })
}
methods: {
  ...mapActions({
    // action方法
  })
}
```















