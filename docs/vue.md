#### 写 `React/Vue` 项目时为什么要在组件中写 `key`，其作用是什么？
`key` 的作用是为了在 `diff` 算法执行时更快的找到对应的节点，提高 `diff` 速度。<br/>
`vue` 和 `react` 都是采用 `diff` 算法来对比新旧虚拟节点，从而更新节点。在 `vue` 的 `diff` 函数中。可以先了解一下 `diff` 算法。<br/>
在交叉对比的时候，当新节点跟旧节点头尾交叉对比没有结果的时候，会根据新节点的 `key` 去对比旧节点数组中的 `key`，从而找到相应旧节点（这里对应的是一个 `key => index` 的 `map` 映射）。如果没找到就认为是一个新增节点。而如果没有 `key`，那么就会采用一种遍历查找的方式去找到对应的旧节点。一种一个 `map` 映射，另一种是遍历查找。相比而言。`map` 映射的速度更快。
#### 对于`MVVM`的理解
`MVVM`分为`Model、View、ViewModel`三者。<br>
`Model` 代表数据模型，数据和业务逻辑都在`Model`层中定义；<br>
`View`代表`UI`视图，负责数据的展示；<br>
`ViewModel`负责监听 `Model` 中数据的改变并且控制视图的更新，处理用户交互操作；<br>
`Model` 和 `View` 并无直接关联，而是通过 `ViewModel` 来进行联系的，`Model` 和 `ViewModel` 之间有着双向数据绑定的联系。因此当 `Model` 中的数据改变时会触发 `View` 层的刷新，`View` 中由于用户交互操作而改变的数据也会在 `Model` 中同步。<br>
这种模式实现了 `Model` 和 `View` 的数据自动同步，因此开发者只需要专注对数据的维护操作即可，而不需要自己操作 `dom`。
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
#### `vue`中子组件为什么不能修改父组件传进来的`props`
为了保证数据的单向流动，便于对数据进行追踪，避免数据混乱
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
#### `Vue`的路由实现：`hash`模式和`history`模式以及`abstract`模式
**hash模式**<br>
在浏览器中符号`“#”`，#以及#后面的字符称之为`hash`，用`window.location.hash`读取。<br>
特点：`hash`虽然在`URL`中，但不被包括在`HTTP`请求中；用来指导浏览器动作，对服务端安全无用，`hash`不会重加载页面。 `hash`模式下，仅`hash`符号之前的内容会被包含在请求中，如 `http://www.xiaogangzai.com`，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 `404` 错误。<br>

**history模式**<br>
`history`采用`HTML5`的新特性；且提供了两个新方法：`pushState()`,`replaceState()`可以对浏览器历史记录栈进行修改，以及`popState`事件的监听到状态变更。 `history`模式下，前端的`URL`必须和实际向后端发起请求的`URL`一致，如 `http://www.xxx.com/items/id`。后端如果缺少对 `/items/id` 的路由处理，将返回 `404` 错误。`Vue-Router` 官网里如此描述：“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 `URL` 匹配不到任何静态资源，则应该返回同一个 `index.html` 页面，这个页面就是你 `app` 依赖的页面。<br>

**abstract模式**<br>
`abstract`模式是使用一个不依赖于浏览器的浏览历史虚拟管理后端。 根据平台差异可以看出，在 `Weex` 环境中只支持使用 `abstract` 模式。

#### keep-alive的实现
作用：实现组件缓存<br>
钩子函数： `activated`组件渲染后调用 `deactivated`组件销毁后调用<br>
原理：`Vue.js`内部将`DOM`节点抽象成了一个个的`VNode`节点，`keep-alive`组件的缓存也是基于`VNode`节点的而不是直接存储`DOM`结构。它将满足条件（`pruneCache与pruneCache`）的组件在`cache`对象中缓存起来，在需要重新渲染的时候再将`vnode`节点从`cache`对象中取出并渲染。<br>
配置属性： `include` 字符串或正则表达式。只有名称匹配的组件会被缓存 `exclude` 字符串或正则表达式。任何名称匹配的组件都不会被缓存 `max` 数字、最多可以缓存多少组件实例<br>
#### `vue` 样式 `scoped`
`scoped`的实现原理: `Vue`中的`scoped`属性的效果主要是通过`PostCss`实现的。
转义前的代码：
```css
<style scoped lang="less">
    .example{
        color:red;
    }
</style>
<template>
    <div class="example">scoped测试案例</div>
</template>
```
转译后:
```css
.example[data-v-5558831a] {
  color: red;
}
<template>
    <div class="example" data-v-5558831a>scoped测试案例</div>
</template>
```
`PostCSS`给一个组件中的所有`dom`添加了一个独一无二的动态属性，给`css`选择器额外添加一个对应的属性选择器，来选择组件中的`dom`,这种做法使得样式只作用于含有该属性的`dom`元素(组件内部的`dom`)。<br>
总结：`scoped`的渲染规则：<br>
给`HTML`的`dom`节点添加一个不重复的`data`属性(例如: `data-v-5558831a`)来唯一标识这个`dom`元素<br>
在每句`css`选择器的末尾(编译后生成的`css`语句)加一个当前组件的`data`属性选择器(例如：`[data-v-5558831a]`)来私有化样式<br>
**scoped穿透**<br>
`sass`和`less`的样式穿透 使用`/deep/`
```css
  外层 /deep/ 第三方组件 {
    样式
  }
  .wrapper /deep/ .swiper-pagination-bullet-active{
    background: #fff;
  }
```
#### slot
一般情况下`v-slot`只能在`<template>`使用。例如`<template v-slot:header>`<br>
**作用域插槽**<br>
有时让插槽内容能够访问子组件中才有的数据是很有用的。例如，设想一个带有如下模板的`<current-user>`组件：
```html
<span>
  <slot>{{ user.lastName }}</slot>
</span>
```
我们想让它的后备内容显示用户的名，以取代正常情况下用户的姓，如下：
```html
<current-user>
  {{ user.firstName }}
</current-user>
```
然而上述代码不会正常工作，因为只有 `<current-user>` 组件可以访问到 `user` 而我们提供的内容是在父级渲染的。<br>
为了让 `user` 在父级的插槽内容中可用，我们可以将 `user` 作为 `<slot>` 元素的一个特性绑定上去：<br>
```html
<span>
  <slot v-bind:user="user">
    {{ user.lastName }}
  </slot>
</span>
```
绑定在 `<slot>` 元素上的特性被称为插槽 `prop`。现在在父级作用域中，我们可以给 `v-slot` 带一个值来定义我们提供的插槽 `prop` 的名字：
```html
<current-user>
  <template v-slot:default="slotProps">
    {{ slotProps.user.firstName }}
  </template>
</current-user>
```
#### `vue.js`中`this`为什么可以访问属性的属性
因为`el、data、computed`都应该理解为`Vue`对象的声明对象内容的关键字，而不是它的直接属性。<br>
那么在`data`声明的就是它（`vm`本身）的数据属性，在`computed`中声明的就是它的计算属性，在`methods`中声明的就是它的方法。


#### `vuex`是什么？怎么使用？哪种功能场景使用它？
`vuex`就是一个仓库，仓库里放了很多对象。其中`state`就是数据源存放地，对应于一般`vue`对象里面的`data`<br>
`state`里面存放的数据是响应式的，`vue` 组件从 `store` 读取数据，若是 `store` 中的数据发生改变，依赖这项数据的组件也会发生更新<br/>
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
#### `Vuex`的设计思想
每一个`Vuex`里面有一个全局的`Store`，包含着应用中的状态`State`，这个`State`只是需要在组件中共享的数据，不用放所有的`State`，没必要。这个`State`是单一的，和 `Redux` 类似，所以，一个应用仅会包含一个 `Store` 实例。单一状态树的好处是能够直接地定位任一特定的状态片段，在调试的过程中也能轻易地取得整个当前应用状态的快照。<br>
`Vuex`通过 `store` 选项，把 `state` 注入到了整个应用中，这样子组件能通过 `this.$store` 访问到 `state`了。
```js
const app = new Vue({
  el: '#app',
  // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
  store,
  components: { Counter },
  template: `
    <div class="app">
      <counter></counter>
    </div>
  `
})
const Counter = {
  template: `<div>{{ count }}</div>`,
  computed: {
    count () {
      return this.$store.state.count
    }
  }
}
```
`Mutation`<br>
显而易见，`State` 不能直接改，需要通过一个约定的方式，这个方式在 `Vuex` 里面叫做 `mutation`，更改 `Vuex` 的 `store` 中的状态的唯一方法是提交 `mutation`。`Vuex` 中的 `mutation` 非常类似于事件：每个 `mutation` 都有一个字符串的 事件类型 (`type`) 和 一个 回调函数 (`handler`)。
```js
const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```
触发 `mutation` 事件的方式不是直接调用，比如 `increment(state)` 是不行的，而要通过 `store.commit` 方法：
```js
store.commit('increment')
```
`mutation` 都是同步事务。<br/>

`Action`<br>
到这里又该处理异步这块儿了。`Vuex` 加入了 `Action` 这个东西来处理异步，`Vuex`的想法是把同步和异步拆分开，异步操作想咋搞咋搞，但是不要干扰了同步操作。
#### 聊聊 `Vue` 的双向数据绑定，`Model` 如何改变 `View`，`View` 又是如何改变 `Model` 的
`VM` 主要做了两件事情：

从 `M` 到 `V` 的映射`（Data Binding）`，这样可以大量节省你人肉来 `update View` 的代码。`Object.defineProperty`<br>
从 `V` 到 `M` 的事件监听`（DOM Listeners）`，这样你的`Model` 会随着 `View` 触发事件而改变<br>
从 `V` 到 `M` 主要由两类（ 虽然本质上都是监听 `DOM` ）构成，一类是用户自定义的 `listener`， 一类是 `VM` 自动处理的含有 `value` 属性元素的 `listener`<br>
第一类类似于你在 `Vue` 里用 `v-on` 时绑定的那样，`VM` 在实例化得时候可以将所有用户自定义的 `listener` 一次性代理到根元素上，这些 `listener` 可以访问到你的 `model` 对象，这样你就可以在 `listener` 中改变 `model`<br>
第二类类似于对含有 `v-model` 与 `value` 元素的自动处理，我们期望的是例如在一个输入框内,输入值，那么我与之对应的 `model` 属性 `message` 也会随之改变，相当于 `VM` 做了一个默认的 `listener`，它会监听这些元素的改变然后自动改变 `model`<br>
#### 双向绑定和 `vuex` 是否冲突
当在严格模式中使用 `Vuex` 时，在属于 `Vuex` 的 `state` 上使用 `v-model` 会比较棘手：
```html
<input v-model="obj.message">
```
假设这里的 `obj` 是在计算属性中返回的一个属于 `Vuex store` 的对象，在用户输入时，`v-model` 会试图直接修改 `obj.message`。在严格模式中，由于这个修改不是在 `mutation` 函数中执行的, 这里会抛出一个错误。<br>
用“`Vuex` 的思维”去解决这个问题的方法是：给 `<input>` 中绑定 `value`，然后侦听 `input` 或者 `change` 事件，在事件回调中调用 `action`:
```html
<input :value="message" @input="updateMessage">
```
```js
computed: {
  ...mapState({
    message: state => state.obj.message
  })
},
methods: {
  updateMessage (e) {
    this.$store.commit('updateMessage', e.target.value)
  }
}
```
```js
mutations: {
  updateMessage (state, message) {
    state.obj.message = message
  }
}
```
#### `Vue` 的响应式原理中 `Object.defineProperty` 有什么缺陷？为什么在 `Vue3.0` 采用了 `Proxy`，抛弃了 `Object.defineProperty`？
`Object.defineProperty`无法监控到数组下标的变化，导致通过数组下标添加元素，不能实时响应；无法监控到对象属性的添加和删除；<br>
`Object.defineProperty`只能劫持对象的属性，从而需要对每个对象，每个属性进行遍历，如果，属性值是对象，还需要深度遍历。`Proxy`可以劫持整个对象，并返回一个新的对象。<br>
`Proxy`不仅可以代理对象，还可以代理数组。还可以代理动态增加的属性。
#### `vue` 中父组件和子组件的生命周期函数的执行顺序
**加载渲染过程**<br>
父 `beforeCreate` -> 父 `created` -> 父 `beforeMount` -> 子 `beforeCreate` -> 子 `created` -> 子 `beforeMount` -> 子 `mounted` -> 父 `mounted` 

**子组件更新过程**<br>
父 `beforeUpdate` -> 子 `beforeUpdate` -> 子 `updated` -> 父 `updated`

**父组件更新过程**<br>
父 `beforeUpdate` -> 父 `updated`

**销毁过程**
父 `beforeDestroy` -> 子 `beforeDestroy` -> 子 `destroyed` -> 父 `destroyed`
#### `input` 如何处理中文输入
中文输入问题，其实看过`elementui`框架源码的童鞋都应该知道，`elementui`是通过`compositionstart & compositionend`做的中文输入处理。 相关代码：
```html
<input
ref="input"
@compositionstart="handleComposition"
@compositionupdate="handleComposition"
@compositionend="handleComposition"
>
```
这3个方法是原生的方法，这里简单介绍下，官方定义如下`compositionstart` 事件触发于一段文字的输入之前（类似于 `keydown` 事件，但是该事件仅在若干可见字符的输入之前，而这些可见字符的输入可能需要一连串的键盘操作、语音识别或者点击输入法的备选词） 简单来说就是切换中文输入法时在打拼音时(此时`input`内还没有填入真正的内容)，会首先触发`compositionstart`，然后每打一个拼音字母，触发`compositionupdate`，最后将输入好的中文填入`input`中时触发`compositionend`。触发`compositionstart`时，文本框会填入 “虚拟文本”（待确认文本），同时触发`input`事件；在触发`compositionend`时，就是填入实际内容后（已确认文本）,所以这里如果不想触发`input`事件的话就得设置一个`bool`变量来控制。
#### `router` 里的 `<Link>` 标签和 `a` 标签有什么区别
有`onclick`那就执行`onclick`<br>
`click`的时候阻止`a`标签默认事件（这样子点击`123`就不会跳转和刷新页面）<br>
再取得跳转`href`（即是`to`），用`history`（前端路由两种方式之一，`history` & hash）跳转，此时只是链接变了，并没有刷新页面
#### `vue` 在 `v-for` 时给每项元素绑定事件需要用事件代理吗？为什么
没有发现 `vue` 会自动做事件代理，但是一般给 `v-for` 绑定事件时，都会让节点指向同一个事件处理程序（第二种情况可以运行，但是 `eslint` 会警告），一定程度上比每生成一个节点都绑定一个不同的事件处理程序性能好，但是监听器的数量仍不会变，所以使用事件代理会更好一点。
#### `vue diff`算法原理
**virtual DOM和真实DOM的区别？**<br>
`virtual DOM` 是将真实的`DOM`的数据抽取出来，以对象的形式模拟树形结构
```html
<div>
    <p>123</p>
</div>
```
对应的 `virtual DOM`：
```js
let Vnode = {
    tag: 'div',
    children: [
        {tag: 'p', text: '123'}
    ]
}
```
`VNode` 和 `oldVNode` 都是对象。<br>

**diff的比较方式？**<br>
在采用 `diff` 算法比较新旧节点的时候，比较只会在同层级的进行，不会跨层级比较。
```html
<div>
    <p>123</p>
</div>
<div>
    <span>456</span>
</div>
```
上面的代码会比较同一层级的两个 `div` 以及第二层级的 `p` 和 `span` ，不会拿 `div` 和 `span` 作比较。<br>
<img width="400" src="/Frontend-Interview-Question/images/level.png" />

**diff流程图**<br>
当数据发生改变时， `setter` 方法会让调用 `Dep.notify()` 通知所有订阅者 `Watcher`，订阅者就会调用 `patch` 给真实的 `DOM` 打补丁，更新相应的视图。
<img width="500" src="/Frontend-Interview-Question/images/overflow.png" />

**patch 是怎么打补丁的？(核心部分)**
```js
function patch (oldVnode, vnode) {
    // some code
    if (sameVnode(oldVnode, vnode)) {
        patchVnode(oldVnode, vnode)
    } else {
        const oEl = oldVnode.el // 当前oldVnode对应的真实元素节点
        let parentEle = api.parentNode(oEl)  // 父元素
        createEle(vnode)  // 根据Vnode生成新元素
        if (parentEle !== null) {
            api.insertBefore(parentEle, vnode.el, api.nextSibling(oEl)) // 将新元素添加进父元素
            api.removeChild(parentEle, oldVnode.el)  // 移除以前的旧元素节点
            oldVnode = null
        }
    }
    // some code 
    return vnode
}

function sameVnode (a, b) {
  return (
    a.key === b.key &&  // key值
    a.tag === b.tag &&  // 标签名
    a.isComment === b.isComment &&  // 是否为注释节点
    // 是否都定义了data，data包含一些具体信息，例如onclick , style
    isDef(a.data) === isDef(b.data) &&  
    sameInputType(a, b) // 当标签是<input>的时候，type必须相同
  )
}
```
`patch`函数接收两个参数`oldVnode`和`Vnode`分别代表新的节点和之前的旧节点。<br>
判断两节点是否值得比较，值得比较则执行 `patchVnode`<br>
不值得比较则用 `Vnode` 替换 `oldVnode`<br>
如果两个节点都是一样的，那么就深入检查他们的子节点。如果两个节点不一样那就说明`Vnode`完全被改变了，就可以直接替换`oldVnode`。<br>
虽然这两个节点不一样但是他们的子节点一样怎么办？别忘了，`diff`可是逐层比较的，如果第一层不一样那么就不会继续深入比较第二层了。

确认两个节点值得比较之后会对两个子节点指定 `patchVnode` 方法。
```js
patchVnode (oldVnode, vnode) {
    const el = vnode.el = oldVnode.el
    let i, oldCh = oldVnode.children, ch = vnode.children
    if (oldVnode === vnode) return
    if (oldVnode.text !== null && vnode.text !== null && oldVnode.text !== vnode.text) {
        api.setTextContent(el, vnode.text)
    }else {
        updateEle(el, vnode, oldVnode)
        if (oldCh && ch && oldCh !== ch) {
            updateChildren(el, oldCh, ch)
        }else if (ch){
            createEle(vnode) //create el's children dom
        }else if (oldCh){
            api.removeChildren(el)
        }
    }
}
```
这个函数做了以下事情：<br>
找到对应的真实的 `dom` ，称为 `el`<br>
判断 `Vnode` 和 `oldVnode` 是否指向同一个对象，如果是，那么直接 `return`<br>
如果它们都有文本节点且不相等，那么将 `el` 的文本节点设置为 `Vnode` 的文本节点<br>
如果 `oldVnode` 有子节点而 `Vnode` 没有，则删除 `el` 的子节点<br>
如果 `oldVnode` 没有子节点而 `Vnode` 有，则将 `Vnode` 的子节点真实化之后添加到 `el`<br>
如果两者都有子节点，则执行 `updateChildren` 函数比较子节点。

**那么 updateCildren 是什么方法呢？**<br>
这个函数主要做了：<br>
将 `Vnode` 的子节点 `Vch` 和 `oldVnode` 的子节点 `oldCh` 提取出来<br>
`oldCh` 和 `vCh` 各有两个头尾的变量 `StartIdx` 和 `EndIdx`，它们的`2`个变量相互比较，一共有四种比较方式。如果`4`中都没有匹配，如果设置里了 `key` ，就会用 `key` 进行比较，在比较的过程中，变量会往中间靠，一旦 `StartIdx > EndIdx` 表明 `oldCh` 和 `vCh` 至少有一个已经遍历完了，就会结束比较。

<img width="500" src="/Frontend-Interview-Question/images/children.png" />

比如上图中的新旧节点，将它们取出来分别用 `s` 和 `e` 指针指向它们的头 `child` 和尾 `child`

<img width="500" src="/Frontend-Interview-Question/images/oldChvCh.png" />

然后分别对 `oldS、oldE、S、E` 两两做 `sameVnode` 比较，有四种比较方式，当其中两个能匹配上，那么真实 `dom` 中的相应节点会移动到 `Vnode` 相应的位置，比如：<br>
如果是 `oldS` 和 `E` 匹配上了，那么真实 `dom` 中第一个节点会移到最后；<br>
如果是 `oldE` 和 `S` 匹配上了，那么真实 `dom` 中的最后一个节点会移到最前，匹配上的两个指针向中间移动；<br>
如果四种匹配没有一对是成功的，那么遍历 `oldChild`，`S` 挨个和他们匹配，匹配成功就在真实 `dom` 中将成功的节点移到最前面。例如：

<img width="500" src="/Frontend-Interview-Question/images/diff1.png" />

上面的图表示 旧的`oldVnode` 中有 `a b d` 三个节点，新的`Vnode`中有 `a c d b` 四个节点。然后开始比较：<br>
第一步：<br>
```js
 oldS = a , oldE = d;
 S = a , E = b;
```
`oldS` 和 `S` 匹配，则将 真实 `dom` 中的 `a` 节点 放到第一个，这里本来就是第一个，所以就不用管了，此时 `dom` 的位置为  `a b d`

第二步：<br>
```js
 oldS = b, oldE = d;
 S = c, E = b
```
`oldS` 和 `E` 匹配，将原本的 `b` 节点移动到最后，因为 `E` 是最后一个节点，它们位置要一致。此时 `dom` 的位置为 `a d b`

第三步：<br>
```js
oldS = d, oldE = d;
S = c, E = d;
```
`oldE` 和 `E` 匹配，位置不变此时 `dom` 位置为 `a d b`

第四步：<br>
```js
oldS++;
oldE--;
oldS > oldE
```
表明旧的 `oldCh` 遍历完成。就将剩余的 `vCh` 节点根据自己的 `index` 插入到真实的 `dom`在中去，此时位置为 `a c d b`，一次模拟完成。<br>
结束条件有两个:<br>
一个是 `oldS > oldE` 表示 `oldCh` 先遍历完，那么就将多余的 `vCh` 根据 `index` 添加到 `dom` 中去<br>
`S > E` 表示 `vCh`遍历完，那么就在真实 ``dom` 中将区间为 `[oldS, oldE]` 的多余及诶单删掉


<img width="500" src="/Frontend-Interview-Question/images/diff2.png" />

上图中`2`和`3`的顺序应该是反的。

<img width="500" src="/Frontend-Interview-Question/images/diff3.png" />

#### vue 和 react diff 算法的区别
vue的diff算法：简单来说，就是vue的diff算法在对新老虚拟daom进行对比时，是从节点的两侧向中间对比；如果节点的key值与元素类型相同，属性值不同，就会认为是不同节点，就会删除重建；<br>
react的diff算法：react的diff算法在对新老虚拟dom进行对比是，是从节点左侧开始对比，就好比将新老虚拟dom放入两个栈中，一对多依次对比；如果节点的key值与元素类型相同，属性值不同，react会认为是同类型节点，只是修改节点属性

#### `computed` 和 `watch` 的区别
`computed` 监控的数据在 `data` 中没有声明；<br/>
`computed`： 是计算属性，依赖其它属性值，并且 `computed` 的值有缓存，只有它依赖的属性值发生改变，下一次获取 `computed` 的值时才会重新计算 `computed` 的值；<br>
`computed` 不支持异步，当 `computed` 中有异步操作时，无法监听数据的变化；<br>
`watch`监测的数据必须在 `data` 中声明或 `props` 中数据；<br>
`watch`支持异步操作；<br>
`watch`没有缓存，页面重新渲染时，值不改变时也会执行<br>

#### watch的实现原理
对 `watch` 每个属性创建一个 `watcher`，触发 `data[key]` 的 `get` 方法, 进行依赖收集; 当 `data[key]` 发生变动时触发 `set` 方法, 通知所有收集的依赖 `watcher` , 触发收集的 `watcher` , 执行 `watch` 中的监听函数。

#### 父组件可以监听到子组件的生命周期吗？
```js
// Parent.vue
<Child @mounted="doSomething"/>
    
// Child.vue
mounted() {
  this.$emit("mounted");
}
```
更简单的方式可以在父组件引用子组件时通过 @hook 来监听即可，如下所示：
```js
//  Parent.vue
<Child @hook:mounted="doSomething" ></Child>

doSomething() {
   console.log('父组件监听到 mounted 钩子函数 ...');
},
    
//  Child.vue
mounted(){
   console.log('子组件触发 mounted 钩子函数 ...');
},
    
// 以上输出顺序为：
// 子组件触发 mounted 钩子函数 ...
// 父组件监听到 mounted 钩子函数 ...
```
当然 `@hook` 方法不仅仅是可以监听 `mounted`，其它的生命周期事件，例如：`created，updated` 等都可以监听。
#### 谈谈你对 `keep-alive` 的了解？
`keep-alive` 是 `Vue` 内置的一个组件，可以使被包含的组件保留状态，避免重新渲染。<br>
其有以下特性：<br>
一般结合路由和动态组件一起使用，用于缓存组件；<br>
提供 `include` 和 `exclude` 属性，两者都支持字符串或正则表达式， `include` 表示只有名称匹配的组件会被缓存，`exclude` 表示任何名称匹配的组件都不会被缓存 ，其中 `exclude` 的优先级比 `include` 高；<br>
对应两个钩子函数 `activated` 和 `deactivated` ，当组件被激活时，触发钩子函数 `activated`，当组件被移除时，触发钩子函数 `deactivated`。
#### 组件中 `data` 为什么是一个函数？
因为组件是用来复用的，且 `JS` 里对象是引用关系，如果组件中 `data` 是一个对象，那么这样作用域没有隔离，子组件中的 `data` 属性值会相互影响，如果组件中 `data` 选项是一个函数，那么每个实例可以维护一份被返回对象的独立的拷贝，组件实例之间的 `data` 属性值不会互相影响；而 `new Vue` 的实例，是不会被复用的，因此不存在引用对象的问题。
#### 实现 `vue` 中的 `on,emit,off,once`，手写代码。
```js
const EventEmiter = function() {
  this._events = {};
}
EventEmiter.prototype.on = function(event, cb) {
  if(Array.isArray(event)) {
    event.forEach(item => {
      this.on(item, cb)
    });
  } else {
    (this._events[event] || (this._events[event] = [])).push(cb);
  }
  return this;
}
EventEmiter.prototype.off = function(event, cb) {
  if(!arguments.length) {
    this._events = null;
    return this;
  }
  if(Array.isArray(event)) {
    event.forEach(item => {
      this.off(item, cb);
    })
    return this;
  }
  if(!cb) {
    this._events[event] = null;
    return this;
  }
  if(cb) {
    let cbs = this._events[event];
    let i = cbs.length;
    while(i--) {
      if(cb === cbs[i] || cb === cbs[i].fn) {
        cbs.splice(i, 1);
        break;
      }
    }
    return this;
  }
}
EventEmiter.prototype.once = function(event, cb) {
  this.on(event, function() {
    this.off(event, cb);
    cb.apply(this, arguments)
  });
  return this;
}
EventEmiter.prototype.emit = function(event) {
  let cbs = this._events[event];
  let args = Array.prototype.slice.call(arguments, 1);
  if(cbs) {
    cbs.forEach(item => {
      item.apply(this, args);
    })
  }
}
```
#### `Data、Observe、Dep`和`Watcher`之间的关系
`Data`通过`Observe`转换成`getter/setter`的形式来追踪变化。当外界通过`watcher`读取数据时，会触发`getter`从而将`watcher`添加到依赖中。当数据发生了变化时， 会触发`setter`，从而向`Dep`中的依赖(`watcher`)发送通知。`watcher`接收到通知后，会向外界发送通知，变化通知到外界后可能触发视图更新，也有可能触发用户的某个回调函数等。
#### `vue`里的`Runtime Only`和`Runtime+Compiler`
官网解释：如果你需要在客户端编译模板 (比如传入一个字符串给 `template` 选项，或挂载到一个元素上并以其 `DOM` 内部的 `HTML` 作为模板)，就将需要加上编译器。<br>

**Runtime Only**<br>
我们在使用 `Runtime Only` 版本的 `Vue.js` 的时候，通常需要借助如 `webpack` 的 `vue-loader` 工具把 `.vue` 文件编译成 `JavaScript`，因为是在编译阶段做的，所以它只包含运行时的 `Vue.js` 代码，因此代码体积也会更轻量。 在将 `.vue` 文件编译成 `JavaScript`的编译过程中会将组件中的`template`模板编译为`render`函数，所以我们得到的是`render`函数的版本。所以运行的时候是不带编译的，编译是在离线的时候做的。<br>

**Runtime+Compiler**<br>
我们如果没有对代码做预编译，但又使用了 `Vue` 的 `template` 属性并传入一个字符串或挂载到一个元素上并以其 `DOM` 内部的 `HTML` 作为模板，则需要在客户端编译模板。
#### `Vue`组件全局注册和局部注册使用及原理
**全局注册实例**
```js
Vue.component('app', App)
```
**局部注册实例**
```js
components: {
  App
}
```
下面我们进入两种注册方式的源码实现，首先进入`Vue.component`
```js
Vue[type] = function (
  id: string,
  definition: Function | Object
): Function | Object | void {
  if (!definition) {
    return this.options[type + 's'][id]
  } else {
    /* istanbul ignore if */
    if (process.env.NODE_ENV !== 'production' && type === 'component') {
      validateComponentName(id)
    }
    if (type === 'component' && isPlainObject(definition)) {
      definition.name = definition.name || id
      definition = this.options._base.extend(definition)
    }
    if (type === 'directive' && typeof definition === 'function') {
      definition = { bind: definition, update: definition }
    }
    this.options[type + 's'][id] = definition
    return definition
  }
}
```
这里是对三种方法的定义`'component', 'directive', 'filter'`，这里我们要说的是`component`，可以看到，当`type = 'component'`时，直接调用了`extend`方法，`extends` 就是对`Vue`对象的扩展，就是一个组件的`Vue`构造函数，拥有`Vue`的所有方法，最后，将这个扩展赋值给了全局`Vue` 的`options` 的`components` 属性中，然后，我们进入到`patch`时解析标签时:
```js
let vnode, ns
  if (typeof tag === 'string') {
    let Ctor
    ns = (context.$vnode && context.$vnode.ns) || config.getTagNamespace(tag)
    if (config.isReservedTag(tag)) {
      // platform built-in elements
      vnode = new VNode(
        config.parsePlatformTagName(tag), data, children,
        undefined, undefined, context
      )
    } else if ((!data || !data.pre) && isDef(Ctor = resolveAsset(context.$options, 'components', tag))) {
      // component
      vnode = createComponent(Ctor, data, context, children, tag)
    } else {
      vnode = new VNode(
        tag, data, children,
        undefined, undefined, context
      )
    }
  } else {
    // direct component options / constructor
    vnode = createComponent(tag, data, context, children)
  }
```
当标签类型为普通标签时，会创建普通类型的标签，如果不是普通标签时我们可以看`else if`里，`Ctor = resolveAsset(context.$options, 'components', tag)` 我们找到这个函数，定义在源码`src/core/util/options.js`
```js
export function resolveAsset (
  options: Object,
  type: string,
  id: string,
  warnMissing?: boolean
): any {
  /* istanbul ignore if */
  if (typeof id !== 'string') {
    return
  }
  const assets = options[type]
  // check local registration variations first
  if (hasOwn(assets, id)) return assets[id]
  const camelizedId = camelize(id)
  if (hasOwn(assets, camelizedId)) return assets[camelizedId]
  const PascalCaseId = capitalize(camelizedId)
  if (hasOwn(assets, PascalCaseId)) return assets[PascalCaseId]
  // fallback to prototype chain
  const res = assets[id] || assets[camelizedId] || assets[PascalCaseId]
  if (process.env.NODE_ENV !== 'production' && warnMissing && !res) {
    warn(
      'Failed to resolve ' + type.slice(0, -1) + ': ' + id,
      options
    )
  }
  return res
}
```
这里的`options`是 `Vue`构造函数的`options`和当前组件`options`的组合，这里是重点，这个`options`是由之前调用的`extends` 方法完成合并的，这也是全局组件和局部组件的区别所在，可以看到代码中是通过这个`options`对象来找注册的组件，三个`if`是对驼峰式和大写式查找的支持，因为之前的`component`方法定义中已经将这个全局注册的组件的名字放到了`Vue`构造函数的`options`中，所以在这里可以通过组合的`options[组建名称]`查找到对应的组件构造函数，因为这个`options`包含`Vue`构造函数`options`和本组件的`options`，所以在任何地方都可以使用全局注册的组件，而局部注册的组件只会在局部组件的配置合并中合并到局部`options`中，当在其他组件的`options`中寻找注册的组件时就会找不到，所以局部注册的组件只存在于局部`options`中，下面我们看下局部`options`的合并:
```js
export function initInternalComponent (vm: Component, options: InternalComponentOptions) {
  const opts = vm.$options = Object.create(vm.constructor.options)
  // doing this because it's faster than dynamic enumeration.
  const parentVnode = options._parentVnode
  opts.parent = options.parent
  opts._parentVnode = parentVnode

  const vnodeComponentOptions = parentVnode.componentOptions
  opts.propsData = vnodeComponentOptions.propsData
  opts._parentListeners = vnodeComponentOptions.listeners
  opts._renderChildren = vnodeComponentOptions.children
  opts._componentTag = vnodeComponentOptions.tag

  if (options.render) {
    opts.render = options.render
    opts.staticRenderFns = options.staticRenderFns
  }
}
```
这里是局部组件`options`合并的代码，局部注册的`components`属性便会合并到`options`中，所以在当前`templete`中的`component`标签便可以在此组件的`options`中找到对应的构造函数从而构造出对应的组件`vnode`。<br>
所以`options`就是组件的构造函数的属性包括`data`啊，`computed`啊，`props`啊，`component`那些，因为组件树的底层`options`都会包含`Vue`构造函数`options`，所以，在`Vue`中注册的`component`可以在任何地方访问到。
#### `vue`中`$nestTick`的原理
`nextTick`接收一个回调函数作为参数，作用是将回调延迟到下次`DOM`更新周期之后执行。下次`DOM`更新周期的意思其实就是下次微任务执行时更新`DOM`,而`$nextTick`其实是将回调添加到微任务中。<br>
在`vue.js`中，当状态发生变化时，`watcher`会得到通知，然后触发虚拟`DOM`的渲染流程。而`watcher`触发渲染这个操作不是同步的，而是异步的。`vue.js`中有一个队列，每当需要渲染时，会将`watcher`推送到这个队列中，在下次事件循环中再让`watcher`触发渲染的流程。

#### 使用`nextTick`如果环境不支持`promise`
`vue`的降级策略: `（micro-task）promise -> MutationObserver ->（macro-task） setTimeout`

想要要创建一个新的 `job`，优先使用 `Promise`，如果浏览器不支持，再尝试 `MutationObserver`。实在不行，只能用 `setTimeout` 创建 `task` 了。

#### `MutationObserver`
`MutationObserver` 是 `html5` 新加的一个功能，其功能是监听`dom`节点的变动，在所有 `dom` 变动完成后，执行回调函数。<br>
具体有一下几点变动的监听：<br>
`childList`：子元素的变动<br>
`attributes`：属性的变动<br>
`characterData`：节点内容或节点文本的变动<br>
`subtree`：所有下属节点（包括子节点和子节点的子节点）的变动
#### `vue` 中 更新了数据但是不更新视图的情况
```html
<p>a属性：{{data.a}}</p>
<p>b属性：{{data.b}}</p>
```
```js
data: {
  b: 1
}
```
直接修改：
```js
this.data.a = 2  // 这样肯定不会生效
this.$set(this.data, 'a', 2)  // 当执行了上面的代码后，也就是直接添加了a属性后，再次$set的时候，也会失效
this.data = {a: 2, b: 1} //直接修改整个data是可以被监听的
```
其次还有就是数组中：使用数组下标修改值或者添加值视图都不会更新，因为`vue`对数组原型上的一些操作方法进行了覆盖。

#### `v-for`中设置`key`为`index`有什么问题
当有列表数据的增删的时候，会出现列表重新渲染的问题，第一点是影响性能，第二点是会导致`index`和预期不一致，比如删除`delete(index)`方法，导致`bug`
#### 指令的原理
在模板解析阶段，将指令解析到`AST`,然后使用`AST`生成代码字符串的过程中实现某些内置指令的功能，最后在虚拟`DOM`渲染的过程中触发自定义指定的钩子函数使指令生效。

#### `v-if`指令的原理
模板编译阶段实现，在代码生成时，通过生成一个特殊的代码字符串来实现指令的功能。<br>
在编译成`AST`后，会有一个类似`has`的变量来确定最终创建哪个节点。
#### `vue`整个渲染过程是什么样的？
1、把模板编译为`render`函数<br>
2、实例进行挂载, 根据根节点`render`函数的调用，递归的生成虚拟`dom`<br>
3、对比虚拟`dom`，渲染到真实`dom`<br>
4、组件内部`data`发生变化，组件和子组件引用`data`作为`props`重新调用`render`函数，生成虚拟`dom`, 返回到步骤`3`<br>

#### 组件的设计
我们知道，只要是组件，就需要在引用的时候与 `view` 或者其他组件进行相关的交互，即 `props` 传值，`$emit` 触发事件，
使用 `$refs` 调用组件方法等，与写在同一个文件相比，耗费的精力明显更多。那为什么需要拆分出组件呢？我认为有两种目的：
复用和隔离。<br>

组件封装有一定的不确定性，更多的时候是在几个方面做权衡，并且随着业务的不断变化中，可能还会面临一些调整和重构。<br>
组件化开发的意义有很多，并不只有复用：<br>
1、组件化是对实现的分层，是更有效地代码组合方式<br>
2、组件化是对资源的重组和优化，从而使项目资源管理更合理<br>
3、组件化有利于单元测试<br>
4、组件化对重构较友好<br>
**组件设计的原则**<br>
单一职责: 把数据处理等带有副作用的工作放在父组件中，而子组件只进行展示或操作，通过事件的方式让父组件进行处理，
保证逻辑归一，后续维护也更为方便。或者使用 `slot` 等类似高阶组件的方式来简化当前组件的内容。<br>

无副作用/引用透明: 和纯函数类似，设计的一个组件不应该对父组件产生副作用，从而达到引用透明（引用多次不影响结果）。
数据操作前必须进行复制。比如需要添加额外的键值，或者需要对数组类型的数据进行操作，会对原始数据产生影响。<br>

入口和出口正确性检查: 在 `props` 时不使用字符串型，而是为它定义详细的类型, 并为它设置默认值<br>
```js
['name'] // 不规范写法
{
  name: {
    type: String,
    default: undefined
  }
}
```
组件划分颗粒度: 组件拆分出来之后，拆成几层或者是拆成几块，影响文件的数量。如果层级比较多，各种 `props` 传递，事件传递，维护成本比较高。<br>
举例：如果是一个二级的列表，即有多个一级列表，一级列表各有一级列表，这个时候应该怎么拆分呢？<br>
按单一原则，我们可能需要拆分成以下几个：一级列表卡片本身，二级列表卡片，二级列表承载组件，一级列表承载组件。
这种划分，组件是三级，两块，数据的传递就会比较困难。如果一级卡片列表不复杂，我们可以将几个 `v-for` 与组件本身合并，
即一级列表承载组件+一级列表卡片+二级列表卡片，二级列表卡片。这种处理方式保证所有的数据处理在第一层上，二级卡片只做渲染，保证逻辑处理集中在一个组件，维护也比较方便。当然，如果一级卡片非常复杂，或者数据需要大量的处理，需要根据情况把最细的进行合并。<br>

新功能下添加新属性/新文件: 对于通用类型组件，我们要求它尽可能的短小精悍，调用起来更为简单，所以不能设计太多的参数。基础组件库不能符合这个要求，主要是因为基础组件库需要尽可能增加普适性，不会因为没有某个常用的属性，导致该组件需要复制一份重写，再加上日积月累的 `pull request`，属性和参数必然会越来越多。而我们在业务中使用，完全不需要这么多的配置，如果有重大差别，重新复制一份，对于后续的维护反而更方便。<br>
所以是否新增加属性还是拷贝一份，是根据后续该组件是否会产生比较大的发展方向差异来决定的。

**Vue组件设计:**<br>
*向下传值*<br>
向下传值就是父级传给子级数据。前面已经提到了，在 `props`传值尽量对传入数据进行类型校验，保证尽快发现问题。除此之外，也有一些注意事项。
传值类型如果是引用类型的 `Object` 类型，那么尽量给它默认值，防止报错。<br>
*向上传值*<br>
`Vue 2.0` 需要使用 `$emit` 进行事件向上冒泡, 父组件进行事件的监听就可以进行处理。

*兼容 v-model*<br>
在如今，【双向绑定】是主流前端框架（`vue、react、ng`等）的必备特性，而组件中的很大一块，就是表单组件。所以若能支持 `v-model` 的特性，会使组件使用起来拥有和原生组件近似的感觉，这个也就是上面提到的【感觉自然】。
通过 `vue` 的文档，我们发现 `v-model` 其实就是一种【语法糖】，其本质是传递一个 `value` 属性，并提供一个 `input` 事件处理器。

*兼容原生事件*<br>
原生的事件会基于 `HTML` 元素进行冒泡，而 `Vue` 的事件处理默认不会产生冒泡行为。这时候我们需要通过 `$listeners` 来访问组件的事件监听者。看下面的两段代码：
```html
<!-- 父文件 -->
<customer-input-wrapper @focus="handleFocus">
```
下面是组件的结构，正常情况下触发 `input` 的 `focus` 事件是不会调用 `handleFocus` 方法的（除非显示 emit 事件），所以我们需要在 `input` 上监听 `$listeners`，写法如下：
```html
<!-- 组件 -->
<div>
  <input v-on='$listeners' />
</div>
```
*组件属性的转移*<br>
一般来说，添加在组件上的属性，会自动赋给组件的根元素。有些时候我们觉得这种默认行为没问题，但也有时候却不能令人满意，比如我想把 `main` 样式类赋给 `input` 元素。这时候需要配合 `inheritAttrs` 和 `$attrs` 两个特性。用法如下：
```js
// 在组件定义里
export default {
  inheritAttrs: false,
  // ...
}
```
```html
<!-- 在组件模板里 -->
<div class="main">
  <input v-on='$listeners' v-bind="$attrs" />
</div>
```

#### Vue2 和 Vue3 中的 v-model 有什么差异？
`vue3` 默`prop`与`event`为：`modelValue`和`update:modelValue`；`vue2` 中则是：`value`和`input`；
`vue3` 中直接通过 `v-model` 后面参数`v-model:msg`来指定属性名，并且支持绑定多个 `v-model`；而 `vue2` 中通过子组件的`model`属性中的`prop`值和`event`值来指定属性名和事件名



