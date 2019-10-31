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

#### `computed` 和 `watch` 的区别
`computed`： 是计算属性，依赖其它属性值，并且 `computed` 的值有缓存，只有它依赖的属性值发生改变，下一次获取 `computed` 的值时才会重新计算 `computed` 的值；<br>
`watch`： 更多的是「观察」的作用，类似于某些数据的监听回调 ，每当监听的数据变化时都会执行回调进行后续操作；
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
      this.off(item ,cb);
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
      cbs[i].apply(this, args);
    })
  }
}
```
#### 单链表
```js
function LinkedList() {
  let Node = function(element) {
    this.element = element;
    this.next = null;
  }
  this.length = 0;
  this.head = null;
  //向列表尾部添加一个新的项
  //第一个场景：向为空的列表添加一个元素。当我们创建一个LinkedList对象时，head会指向null,如果head元素为null（列表为空），就意味着在向列表添加第一个元素。
  // 因此要做的就是让head元素指向node元素。下一个node元素将会自动成为null
  //第二个场景：向不为空的列表尾部添加元素，首先要找到最后一个元素，我们只知道第一个元素，所以要循环列表，找到最后一项。
  // 循环列表时，当current.next元素为null时，就表示达到列表尾部了。
  this.append = function(element) {
    let node = new Node(element);
    let current;
    if(head === null) {
      // 链表中第一个节点
      head = node;
    } else {
      current = head;
      // 循环链表，直到找到最后一项
      while(current.next) {
        current = current.next;
      }
      current.next = node;
    }
    length++;
  }
  this.removeAt = function(position) {
    if(position > -1 && position < length) {
      let current = head;
      let previous;
      let index = 0;
      // 如果是第一项，直接用head指向current.next
      if(position === 0) {
        head = current.next;
      } else {
        // 如果需要移除链表最后一项或者中间项，需要迭代链表
        while(index++ < position) {
          previous = current;
          current = current.next
        }
        previous.next = current.next;
      }
      length--;
      return current.element;
    } else {
      return null;
    }
  }
  this.insert = function(position, element) {
    if(position >= 0 && position <= length) {
      let node = new Node(element);
      let current = head;
      let previous;
      let index = 0;
      // 在第一个位置插入,只需要把新增的node节点的next指向head就可以
      if(position === 0) {
        node.next = current;
        head = node;
      } else {
        while(index++ < position) {
          previous = current;
          current = current.next;
        }
        node.next = current;
        previous.next = node;
      }
      length++;
      return true;
    } else {
      return false;
    }
  }
  this.remove = function(element) {
    let index = this.indexOf(element);
    return this.removeAt(index);
  }
}
```
#### 双向链表
```js
function DoublyLinkedList() {
  let Node = function(element) {
    this.element = element;
    this.next = null;
    this.prev = null;
  }
  let length = 0;
  let head = null;
  let tail = null;
  this.insert = function(position, element) {
    if(position >= 0 && position <= length) {
      let node = new Node(element);
      let current = head;
      let previous;
      let index = 0;
      if(position === 0) {
        if(!head) {
          head = node;
          tail = node;
        } else {
          node.next = current;
          current.prev = node;
          head = node;
        }
      } else if(position === length) {
        current = tail;
        current.next = node;
        node.prev = current;
        tail = node;
      } else {
        while(index++ < position) {
          previous = current;
          current = current.next;
        }
        node.next = current;
        previous.next = node;
        current.prev = node;
        node.prev = previous;
      }
      length++;
      return true;
    } else {
      return false
    }
  }
  this.removeAt = function(position) {
    if(position > -1 && position < length) {
      let current = head;
      let previous;
      let index = 0;
      if(position === 0) {
        head = current.next;
        if(length === 1) {
          tail = null
        } else {
          head.prev = null
        }
      } else if(position === length - 1) {
        current = tail;
        tail = current.prev;
        tail.next = null;
      } else {
        while(index++ > position) {
          previous = current;
          current = current.next;
        }
        previous.next = current.next;
        current.next.prev = current;
      }
      length--;
      return current.element;
    } else {
      return null;
    }
  }
}

```







