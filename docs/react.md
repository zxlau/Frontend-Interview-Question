#### `React`生命周期有哪些，16版本生命周期发生了哪些变化？
**15版本生命周期**
```js
// 初始化阶段：
constructor  // 构造函数
getDefaultProps // props默认值
getInitialState // state默认值
```
```js
// 挂载阶段
componentWillMount // 组件初始化渲染前调用
render // 组件渲染
componentDidMount // 组件挂载到 DOM后调用
```
```js
// 更新阶段 props
componentWillReceiveProps // 组件将要接收新 props前调用
shouldComponentUpdate //组件是否需要更新
componentWillUpdate //组件更新前调用
render // 组件渲染
componentDidUpdate // 组件更新后调用

// 更新阶段 state
shouldComponentUpdate
componentWillUpdate
render
componentDidUpdate
```
```js
// 卸载阶段
componentWillUnMount // 组件卸载前调用
```
**16版本生命周期**
```js
// 初始化阶段
constructor // 构造函数
getDefaultProps // props默认值
getInitialState // state默认值
```
```js
// 挂载阶段
static getDerivedStateFromProps (nextProps,preState)
render
componentDidMount
```
`getDerivedStateFromProps`：组件每次被 `rerender`的时候，包括在组件构建之后(虚拟 `dom` 之后，实际 `dom` 挂载之前)，每次获取新的 `props` 或 `state` 之后；每次接收新的`props` 之后都会返回一个对象作为新的 `state`，返回`null`则说明不需要更新 `state`；配合 `componentDidUpdate`，可以覆盖 `componentWillReceiveProps`的所有用法
```js
// 更新阶段
static getDerivedStateFromProps (nextProps,preState)
shouldComponentUpdate
render
getSnapshotBeforeUpdate(prevProps,prevState)
componentDidUpdate
```
`getSnapshotBeforeUpdate`：触发时间: `update` 发生的时候，在 `render` 之后，在组件 `dom` 渲染之前；返回一个值，作为 `componentDidUpdate` 的第三个参数；配合 `componentDidUpdate`, 可以覆盖 `componentWillUpdate` 的所有用法
```js
// 卸载阶段
componentWillUnmount
```
```js
// 错误处理
componentDidCatch
```
`React16`新的生命周期弃用了 `componentWillMount`、`componentWillReceivePorps`，`componentWillUpdate`新增了 `getDerivedStateFromProps`、`getSnapshotBeforeUpdate`来代替弃用的三个钩子函数。<br/>
注：`React16`并没有删除这三个钩子函数，但是不能和新增的钩子函数混用， `React17`将会删除这三个钩子函数，新增了对错误的处理`（componentDidCatch）`

#### `setState`是同步的还是异步的？
**生命周期和合成事件中**<br/>
在 `React` 的生命周期和合成事件中， `React` 仍然处于他的更新机制中，这时无论调用多少次 `setState`，都会不会立即执行更新，而是将要更新的存入 `_pendingStateQueue`，将要更新的组件存入 `dirtyComponent`。当上一次更新机制执行完毕，以生命周期为例，所有组件，即最顶层组件 `didmount` 后会将批处理标志设置为 `false`。这时将取出 `dirtyComponent`中的组件以及 `_pendingStateQueue` 中的 `state` 进行更新。这样就可以确保组件不会被重新渲染多次。
```js
componentDidMount() {
  this.setState({
    index: this.state.index+1
  })
  console.log(this.state.index)
}
```
所以，如上面的代码，当我们在执行 `setState` 后立即去获取 `state`，这时是获取不到更新后的 `state` 的，因为处于 `React`的批处理机制中， `state`被暂存起来，待批处理机制完成之后，统一进行更新。所以。`setState`本身并不是异步的，而是 `React` 的批处理机制给人一种异步的假象。<br/>

**异步代码和原生事件中**<br/>
```js
componentDidMount() {
  setTimeOut(() => {
    this.setState({
      index: this.state.index+1
    })
    console.log(this.state.index)
  }, 0)
}
```
如上面的代码，当我们在异步代码中调用 `setState` 时，根据 `JavaScript` 的异步机制，会将异步代码先暂存，等所有同步代码执行完毕后在执行，这时 `React` 的批处理机制已经走完，处理标志设被设置为 `false`，这时再调用 `setState`即可立即执行更新，拿到更新后的结果。<br/>
在原生事件中调用 `setState` 并不会出发 `React`的批处理机制，所以立即能拿到最新结果。<br/>

**最佳实践**<br/>
`setState`的第二个参数接收一个函数，该函数会在 `React` 的批处理机制完成之后调用，所以你想在调用 `setState` 后立即获取更新后的值，请在该回调函数中获取。<br/>

**为什么有时连续多次setState只有一次生效？**<br/>
```js
componentDidMount() {
  this.setState({
    index: this.state.index+1
  })
  console.log(this.state.index);
  this.setState({
    index: this.state.index+1
  })
  console.log(this.state.index);
}
```
原因就是 `React`会批处理机制中存储的多个 `setState` 进行合并，来看下 `React` 源码中的 `_assign` 函数，类似于 `Object`的 `assign`：
```js
 _assign(nextState, typeof partial === 'function' ? partial.call(inst, nextState, props, context) : partial);
```
如果传入的是对象，很明显会被合并成一次，所以上面的代码两次打印的结果是相同的：
```js
Object.assign(  nextState,  {index: state.index+ 1},  {index: state.index+ 1})
```
注意， `assign` 函数中对函数做了特殊处理，处理第一个参数传入的是函数，函数的参数 `preState` 是前一次合并后的结果，所以计算结果是准确的：
```js
  componentDidMount() {    
    this.setState((state, props) => ({        
      index: state.index + 1    
    }), () => {      
      console.log(this.state.index);    
    })    
    this.setState((state, props) => ({        
      index: state.index + 1    
    }), () => {      
      console.log(this.state.index);    
    })
  }
```
所以上面的代码两次打印的结果是不同的。
`React`会对多次连续的 `setState` 进行合并，如果你想立即使用上次 `setState` 后的结果进行下一次 `setState`，可以让 `setState` 接收一个函数而不是一个对象。这个函数用上一个 `state` 作为第一个参数，将此次更新被应用时的 `props` 做为第二个参数。<br/>

#### React如何实现自己的事件机制？
简要回答：
- `React` 它并不会把事件处理函数直接绑定到真实的节点上，而是把所有事件绑定到结构的最外层，使用一个统一的事件监听器。
- 这个事件监听器上维持了一个映射来保存所有组件内部的事件监听和处理函数。
- 当组件挂载或卸载时，只是在这个统一的事件监听器上插入或删除一些对象；
- 当事件发生时，首先被这个统一的事件监听器处理，然后在映射里找到真正的事件处理函数并调用。

详细回答，描述过程：<br/>
`React`事件并没有绑定在真实的 `Dom` 节点上，而是通过事件代理，在最外层的 `document` 上对事件进行统一分发。<br/>
组件挂载、更新时：
- 通过 `lastProps、 nextProps`判断是否新增、删除事件分别调用事件注册、卸载方法。
- 调用 `EventPluginHub` 的 `enqueuePutListener` 进行事件存储
- 获取 `document` 对象。
- 根据事件名称（如 `onClick、 onCaptureClick`）判断是进行冒泡还是捕获。
- 判断是否存在 `addEventListener` 方法，否则使用 `attachEvent`（兼容IE）。
- 给 `document` 注册原生事件回调为 `dispatchEvent` (统一的事件分发机制）。<br/>

事件初始化：<br>
- `EventPluginHub` 负责管理 `React` 合成事件的 `callback`，它将 `callback` 存储在 `listenerBank` 中，另外还存储了负责合成事件的 `Plugin`。
- 获取绑定事件的元素的唯一标识 `key`。
- 将 `callback` 根据事件类型，元素的唯一标识 `key` 存储在 `listenerBank` 中。
- `listenerBank` 的结构是： `listenerBank[registrationName][key]`。

触发事件时：<br/>
- 触发 document注册原生事件的回调 dispatchEvent
- 获取到触发这个事件最深一级的元素
- 遍历这个元素的所有父元素，依次对每一级元素进行处理。
- 构造合成事件。
- 将每一级的合成事件存储在 eventQueue事件队列中。
- 遍历 eventQueue。
- 通过 isPropagationStopped判断当前事件是否执行了阻止冒泡方法。
- 如果阻止了冒泡，停止遍历，否则通过 executeDispatch执行合成事件。
- 释放处理完成的事件。

#### 为`何React`事件要自己绑定`this`？
事件处理流程中， `React` 在 `document` 上进行统一的事件分发， `dispatchEvent` 通过循环调用所有层级的事件来模拟事件冒泡和捕获。<br/>
在 `React` 源码中，当具体到某一事件处理函数将要调用时，将调用 `invokeGuardedCallback` 方法。
```js
function invokeGuardedCallback(name, func, a) {  
  try {    
    func(a);  
  } catch (x) {    
    if (caughtError === null) {      
      caughtError = x;    
    }  
  }}
```
事件处理函数是直接调用的，并没有指定调用的组件，所以不进行手动绑定的情况下直接获取到的 `this` 是不准确的，所以我们需要手动将当前组件绑定到 `this` 上或者写箭头函数自动绑定 `this` 到当前组件。

#### 原生事件和`React`事件的区别？
- `React` 事件使用驼峰命名，而不是全部小写。
- 通过 `JSX`, 你传递一个函数作为事件处理程序，而不是一个字符串。
- 在 `React` 中你不能通过返回 `false` 来阻止默认行为。必须明确调用 `preventDefault`。

####  `React`的合成事件是什么？
`React` 根据 `W3C` 规范定义了每个事件处理函数的参数，即合成事件。<br/>
事件处理程序将传递 `SyntheticEvent` 的实例，这是一个跨浏览器原生事件包装器。它具有与浏览器原生事件相同的接口，包括 `stopPropagation()` 和 `preventDefault()`，在所有浏览器中他们工作方式都相同。<br/>
`React`合成的 `SyntheticEvent`采用了事件池，这样做可以大大节省内存，而不会频繁的创建和销毁事件对象。<br/>
另外，不管在什么浏览器环境下，浏览器会将该事件类型统一创建为合成事件，从而达到了浏览器兼容的目的。

#### `React`和原生事件的执行顺序是什么？可以混用吗？
`React`的所有事件都通过 `document` 进行统一分发。当真实 `Dom` 触发事件后冒泡到 `document` 后才会对 `React` 事件进行处理。所以原生的事件会先执行，然后执行 `React` 合成事件，最后执行真正在 `document` 上挂载的事件`React`事件和原生事件最好不要混用。原生事件中如果执行了 `stopPropagation`方法，则会导致其他 `React`事件失效。因为所有元素的事件将无法冒泡到 `document`上，导致所有的 `React`事件都将无法被触发。

####  虚拟`Dom`是什么？
在原生的 `JavaScript` 程序中，我们直接对 `DOM` 进行创建和更改，而 `DOM` 元素通过我们监听的事件和我们的应用程序进行通讯。而 `React` 会先将你的代码转换成一个 `JavaScript` 对象，然后这个 `JavaScript` 对象再转换成真实 `DOM`。这个 `JavaScript` 对象就是所谓的虚拟 `DOM`。当我们需要创建或更新元素时， `React`首先会让这个 `VitrualDom` 对象进行创建和更改，然后再将 `VitrualDom` 对象渲染成真实`DOM`。当我们需要对 `DOM` 进行事件监听时，首先对 `VitrualDom` 进行事件监听， `VitrualDom` 会代理原生的 `DOM` 事件从而做出响应。

#### 虚拟`Dom`比普通`Dom`更快吗？
很多文章说 `VitrualDom` 可以提升性能，这一说法实际上是很片面的。直接操作 `DOM` 是非常耗费性能的，这一点毋庸置疑。但是 `React` 使用 `VitrualDom` 也是无法避免操作 `DOM` 的。如果是首次渲染， `VitrualDom` 不具有任何优势，甚至它要进行更多的计算，消耗更多的内存。`VitrualDom`的优势在于 `React` 的 `Diff` 算法和批处理策略， `React` 在页面更新之前，提前计算好了如何进行更新和渲染 `DOM`。实际上，这个计算过程我们在直接操作 `DOM`时，也是可以自己判断和实现的，但是一定会耗费非常多的精力和时间，而且往往我们自己做的是不如 `React`好的。所以，在这个过程中 `React`帮助我们"提升了性能"。所以，我更倾向于说， `VitrualDom`帮助我们提高了开发效率，在重复渲染时它帮助我们计算如何更高效的更新，而不是它比 `DOM` 操作更快。



