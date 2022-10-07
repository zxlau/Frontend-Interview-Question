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

#### 虚拟`Dom`中的`?typeof`属性的作用是什么？
`ReactElement` 中有一个 `?typeof` 属性，它被赋值为 `REACT_ELEMENT_TYPE`：
```js
var REACT_ELEMENT_TYPE =  (typeof Symbol === 'function' && Symbol.for && Symbol.for('react.element')) ||  0xeac7;
```
可见， `?typeof` 是一个 `Symbol` 类型的变量，这个变量可以防止 `XSS`。
如果你的服务器有一个漏洞，允许用户存储任意 `JSON` 对象， 而客户端代码需要一个字符串，这可能会成为一个问题：
```js
let expectedTextButGotJSON = {  
  type: 'div',  
  props: {    
    dangerouslySetInnerHTML: {      
      __html: '/* put your exploit here */'    },  
    },
  };
  let message = { 
    text: expectedTextButGotJSON 
  };
  <p>{message.text}</p>
```
`JSON` 中不能存储 `Symbol`类型的变量。
`ReactElement.isValidElement` 函数用来判断一个 `React` 组件是否是有效的，下面是它的具体实现。
```js
ReactElement.isValidElement = function (object) {  
  return typeof object === 'object' && object !== null && object.$$typeof === REACT_ELEMENT_TYPE;
};
```
可见 `React` 渲染时会把没有 `?typeof` 标识，以及规则校验不通过的组件过滤掉。

当你的环境不支持 `Symbol` 时， `?typeof` 被赋值为 `0xeac7`，至于为什么， `React` 开发者给出了答案：`0xeac7`看起来有点像 `React`。

#### `React`组件的渲染流程是什么？
- 使用 `React.createElement` 或 `JSX` 编写 `React` 组件，实际上所有的 `JSX` 代码最后都会转换成 `React`.`createElement(...)`， `Babel`帮助我们完成了这个转换的过程。
- `createElement` 函数对 `key` 和 `ref` 等特殊的 `props` 进行处理，并获取`defaultProps`对默认`props`进行赋值，并且对传入的孩子节点进行处理，最终构造成一个`ReactElement`对象（所谓的虚拟 `DOM`）。
- `ReactDOM.render`将生成好的虚拟`DOM`渲染到指定容器上，其中采用了批处理、事务等机制并且对特定浏览器进行了性能优化，最终转换为真实`DOM`。

#### 为什么代码中一定要引入`React`？
`JSX`只是为`React.createElement(component,props,...children)`方法提供的语法糖。所有的`JSX`代码最后都会转换成`React.createElement(...)`， `Babel`帮助我们完成了这个转换的过程。所以使用了`JSX`的代码都必须引入 `React`。

#### 为什么`React`组件首字母必须大写？
`babel`在编译时会判断`JSX`中组件的首字母，当首字母为小写时，其被认定为原生`DOM`标签，`createElement`的第一个变量被编译为字符串；当首字母为大写时，其被认定为自定义组件，`createElement`的第一个变量被编译为对象；

#### `React`在渲染真实`Dom`时做了哪些性能优化？
在 `IE（8-11）`和 `Edge`浏览器中，一个一个插入无子孙的节点，效率要远高于插入一整个序列化完整的节点树。`React`通过 `lazyTree`，在 `IE（8-11）`和 `Edge`中进行单个节点依次渲染节点，而在其他浏览器中则首先将整个大的 `DOM`结构构建好，然后再整体插入容器。并且，在单独渲染节点时， `React`还考虑了 `fragment` 等特殊节点，这些节点则不会一个一个插入渲染。

#### 什么是高阶组件？如何实现？
高阶组件可以看作`React`对装饰模式的一种实现，高阶组件就是一个函数，且该函数接受一个组件作为参数，并返回一个新的组件。
高阶组件`（ HOC）`是 `React`中的高级技术，用来重用组件逻辑。但高阶组件本身并不是 `ReactAPI`。它只是一种模式，这种模式是由 `React` 自身的组合性质必然产生的。
```jsx
function visible(WrappedComponent) {  
  return class extends Component {    
    render() {      
      const { visible, ...props } = this.props;      
      if (visible === false) return null;      
      return <WrappedComponent {...props} />;    
    } 
  }
}
```
上面的代码就是一个 `HOC`的简单应用，函数接收一个组件作为参数，并返回一个新组件，新组建可以接收一个 `visible props`，根据 `visible` 的值来判断是否渲染`Visible`。

我们可以通过以下两种方式实现高阶组件：

**属性代理**<br/>
函数返回一个我们自己定义的组件，然后在 `render`中返回要包裹的组件，这样我们就可以代理所有传入的 `props`，并且决定如何渲染，实际上 ，这种方式生成的高阶组件就是原组件的父组件，上面的函数 `visible`就是一个 `HOC`属性代理的实现方式。
```jsx
function proxyHOC(WrappedComponent) {  
  return class extends Component {    
    render() {      
      return <WrappedComponent {...this.props} />;    
    }
  }
}
```
对比原生组件增强的项：
- 可操作所有传入的 `props`
- 可操作组件的生命周期
- 可操作组件的`static`方法
` 获取`refs`

**反向继承**<br/>
返回一个组件，继承原组件，在`render`中调用原组件的`render`。由于继承了原组件，能通过`this`访问到原组件的 生命周期、`props、state、render`等，相比属性代理它能操作更多的属性。
```jsx
function inheritHOC(WrappedComponent) {  
  return class extends WrappedComponent {    
    render() {      
      return super.render();    
    }  
  }
}
```
对比原生组件增强的项：
- 可操作所有传入的`props`
- 可操作组件的生命周期
- 可操作组件的`static`方法
- 获取`refs`
- 可操作`state`
- 可以渲染劫持

#### `HOC`在业务场景中有哪些实际应用场景？
`HOC`可以实现的功能：
- 组合渲染
- 条件渲染
- 操作 `props`
- 获取 `refs`
- 状态管理
- 操作 `state`
- 渲染劫持
`HOC`在业务中的实际应用场景：
- 日志打点
- 权限控制
- 双向绑定
- 表单校验

#### 高阶组件`(HOC)`和`Mixin`的异同点是什么？
`Mixin`和`HOC`都可以用来解决`React`的代码复用问题。
- `Mixin`可能会相互依赖，相互耦合，不利于代码维护
- 不同的`Mixin`中的方法可能会相互冲突
- `Mixin`非常多时，组件是可以感知到的，甚至还要为其做相关处理，这样会给代码造成滚雪球式的复杂性

而 `HOC`的出现可以解决这些问题：
- 高阶组件就是一个没有副作用的纯函数，各个高阶组件不会互相依赖耦合
- 高阶组件也有可能造成冲突，但我们可以在遵守约定的情况下避免这些行为
- 高阶组件并不关心数据使用的方式和原因，而被包裹的组件也不关心数据来自何处。高阶组件的增加不会为原组件增加负担

#### `Hook`有哪些优势？
- 减少状态逻辑复用的风险<br/>
`Hook`和`Mixin`在用法上有一定的相似之处，但是`Mixin`引入的逻辑和状态是可以相互覆盖的，而多个`Hook`之间互不影响，这让我们不需要在把一部分精力放在防止避免逻辑复用的冲突上。在不遵守约定的情况下使用`HOC`也有可能带来一定冲突，比如 `props`覆盖等等，使用 `Hook`则可以避免这些问题。
- 避免地狱式嵌套<br/>
大量使用 `HOC`的情况下让我们的代码变得嵌套层级非常深，使用 `HOC`，我们可以实现扁平式的状态逻辑复用，而避免了大量的组件嵌套。
- 让组件更容易理解<br/>
在使用 `class`组件构建我们的程序时，他们各自拥有自己的状态，业务逻辑的复杂使这些组件变得越来越庞大，各个生命周期中会调用越来越多的逻辑，越来越难以维护。使用 `Hook`，可以让你更大限度的将公用逻辑抽离，将一个组件分割成更小的函数，而不是强制基于生命周期方法进行分割。
- 使用函数代替`class`<br/>
相比函数，编写一个 `class`可能需要掌握更多的知识，需要注意的点也越多，比如 `this`指向、绑定事件等等。另外，计算机理解一个 `class`比理解一个函数更快。`Hooks`让你可以在 `classes`之外使用更多 `React`的新特性。

#### 为什么选择使用框架而不是原生?
框架的好处:<br/>
组件化: 其中以 `React` 的组件化最为彻底,甚至可以到函数级别的原子组件,高度的组件化可以是我们的工程易于维护、易于组合拓展。<br/>
天然分层: `JQuery` 时代的代码大部分情况下是面条代码,耦合严重,现代框架不管是 `MVC、MVP`还是`MVVM`模式都能帮助我们进行分层，代码解耦更易于读写。<br/>
生态: 现在主流前端框架都自带生态,不管是数据流管理架构还是 `UI` 库都有成熟的解决方案。<br/>
开发效率: 现代前端框架都默认自动更新`DOM`,而非我们手动操作,解放了开发者的手动`DOM`成本,提高开发效率,从根本上解决了`UI`与状态同步问题.

#### 虚拟`DOM`的优劣如何?
优点:<br/>
保证性能下限: 虚拟`DOM`可以经过`diff`找出最小差异,然后批量进行`patch`,这种操作虽然比不上手动优化,但是比起粗暴的`DOM`操作性能要好很多,因此虚拟`DOM`可以保证性能下限
无需手动操作`DOM`: 虚拟`DOM`的`diff`和`patch`都是在一次更新中自动进行的,我们无需手动操作`DOM`,极大提高开发效率
跨平台: 虚拟`DOM`本质上是`JavaScript`对象,而`DOM`与平台强相关,相比之下虚拟`DOM`可以进行更方便地跨平台操作,例如服务器渲染、移动端开发等等<br/>
缺点:<br/>
无法进行极致优化: 在一些性能要求极高的应用中虚拟`DOM`无法进行针对性的极致优化,比如`VScode`采用直接手动操作`DOM`的方式进行极端的性能优化

#### 虚拟`DOM`实现原理?
- 虚拟`DOM`本质上是`JavaScript`对象,是对真实`DOM`的抽象
- 状态变更时，记录新树和旧树的差异
- 最后把差异更新到真正的`dom`中

#### `React`的请求应该放在哪个生命周期中?
`React`的异步请求到底应该放在哪个生命周期里,有人认为在`componentWillMount`中可以提前进行异步请求,避免白屏,其实这个观点是有问题的.
由于`JavaScript`中异步事件的性质，当您启动API调用时，浏览器会在此期间返回执行其他工作。当`React`渲染一个组件时，它不会等待`componentWillMount`它完成任何事情 - `React`继续前进并继续`render`,没有办法“暂停”渲染以等待数据到达。
而且在`componentWillMount`请求会有一系列潜在的问题,首先,在服务器渲染时,如果在 `componentWillMount` 里获取数据，`fetch data`会执行两次，一次在服务端一次在客户端，这造成了多余的请求,其次,在`React 16`进行`React Fiber`重写后,`componentWillMount`可能在一次渲染中多次调用.
目前官方推荐的异步请求是在`componentDidmount`中进行.
如果有特殊需求需要提前请求,也可以在特殊情况下在`constructor`中请求。

#### `React`组件通信如何实现?
`React`组件间通信方式:
- 父组件向子组件通讯: 父组件可以向子组件通过传 `props` 的方式，向子组件进行通讯
- 子组件向父组件通讯: `props`+回调的方式,父组件向子组件传递`props`进行通讯，此`props`为作用域为父组件自身的函数，子组件调用该函数，将子组件想要传递的信息，作为参数，传递到父组件的作用域中
- 兄弟组件通信: 找到这两个兄弟节点共同的父节点,结合上面两种方式由父节点转发信息进行通信
- 跨层级通信: `Context`设计目的是为了共享那些对于一个组件树而言是“全局”的数据，例如当前认证的用户、主题或首选语言,对于跨越多层的全局数据通过`Context`通信再适合不过
- 发布订阅模式: 发布者发布事件，订阅者监听事件并做出反应,我们可以通过引入`event`模块进行通信
- 全局状态管理工具: 借助`Redux`或者`Mobx`等全局状态管理工具进行通信,这种工具会维护一个全局状态中心`Store`,并根据不同的事件产生新的状态

#### `mixin、hoc、render props、react-hooks`的优劣如何？
`Mixin`的缺陷：<br/>
组件与 `Mixin` 之间存在隐式依赖（`Mixin` 经常依赖组件的特定方法，但在定义组件时并不知道这种依赖关系）
多个 `Mixin` 之间可能产生冲突（比如定义了相同的`state`字段）<br/>
`Mixin` 倾向于增加更多状态，这降低了应用的可预测性`（The more state in your application, the harder it is to reason about it.）`，导致复杂度剧增<br/>
隐式依赖导致依赖关系不透明，维护成本和理解成本迅速攀升：<br/>
难以快速理解组件行为，需要全盘了解所有依赖 `Mixin` 的扩展行为，及其之间的相互影响<br/>
组价自身的方法和`state`字段不敢轻易删改，因为难以确定有没有 `Mixin` 依赖它<br/>
`Mixin` 也难以维护，因为 `Mixin` 逻辑最后会被打平合并到一起，很难搞清楚一个 `Mixin` 的输入输出<br/>

`HOC`相比Mixin的优势:<br>
`HOC`通过外层组件通过 `Props` 影响内层组件的状态，而不是直接改变其 `State` 不存在冲突和互相干扰,这就降低了耦合度
不同于 `Mixin` 的打平+合并，`HOC` 具有天然的层级结构（组件树结构），这又降低了复杂度<br/>

`HOC`的缺陷:<br/>
扩展性限制: `HOC` 无法从外部访问子组件的 `State` 因此无法通过`shouldComponentUpdate`滤掉不必要的更新,<br/>
`React` 在支持 `ES6 Class` 之后提供了`React.PureComponent`来解决这个问题<br/>
`Ref` 传递问题: `Ref` 被隔断,后来的`React.forwardRef` 来解决这个问题<br/>
`Wrapper Hell`: `HOC`可能出现多层包裹组件的情况,多层抽象同样增加了复杂度和理解成本<br/>
不可见性: `HOC`相当于在原有组件外层再包装一个组件,你压根不知道外层的包装是啥,对于你是黑盒<br/>

`Render Props`优点:<br/>

上述`HOC`的缺点`Render Props`都可以解决<br/>
`Render Props`缺陷:<br/>
使用繁琐: `HOC`使用只需要借助装饰器语法通常一行代码就可以进行复用,`Render Props`无法做到如此简单<br/>
嵌套过深: `Render Props`虽然摆脱了组件多层嵌套的问题,但是转化为了函数回调的嵌套<br/>

`React Hooks`优点:<br/>
简洁: `React Hooks`解决了`HOC`和`Render Props`的嵌套问题,更加简洁<br/>
解耦: `React Hooks`可以更方便地把 `UI`和状态分离,做到更彻底的解耦<br/>
组合: `Hooks` 中可以引用另外的 `Hooks` 形成新的`Hooks`,组合变化万千<br/>
函数友好: `React Hooks`为函数组件而生,从而解决了类组件的几大问题:<br/>
`this` 指向容易错误<br/>
分割在不同声明周期中的逻辑使得代码难以理解和维护<br/>
代码复用成本高（高阶组件容易使代码量剧增）<br/>

`React Hooks`缺陷:<br/>
额外的学习成本（`Functional Component `与 `Class Component` 之间的困惑）<br/>
写法上有限制（不能出现在条件、循环中），并且写法限制增加了重构成本<br/>
破坏了`PureComponent、React.memo`浅比较的性能优化效果（为了取最新的`props`和`state`，每次`render()`都要重新创建事件处函数）<br/>
在闭包场景可能会引用到旧的`state、props`值<br/>
内部实现上不直观（依赖一份可变的全局状态，不再那么“纯”）<br/>
`React.memo`并不能完全替代`shouldComponentUpdate`（因为拿不到 `state change`，只针对 `props change`）<br/>

#### 你是如何理解`fiber`的?
`React Fiber` 是一种基于浏览器的单线程调度算法.
`React 16`之前 ，`reconcilation` 算法实际上是递归，想要中断递归是很困难的，`React 16` 开始使用了循环来代替之前的递归.
`Fiber`：一种将 `recocilation` （递归 `diff`），拆分成无数个小任务的算法；它随时能够停止，恢复。停止恢复的时机取决于当前的一帧（`16ms`）内，还有没有足够的时间允许计算。

#### 你对 `Time Slice`的理解?
时间分片<br>
- `React` 在渲染（`render`）的时候，不会阻塞现在的线程
- 如果你的设备足够快，你会感觉渲染是同步的
- 如果你设备非常慢，你会感觉还算是灵敏的
- 虽然是异步渲染，但是你将会看到完整的渲染，而不是一个组件一行行的渲染出来
- 同样书写组件的方式
时间分片正是基于可随时打断、重启的`Fiber`架构,可打断当前任务,优先处理紧急且重要的任务,保证页面的流畅运行.

#### `redux`的工作流程?
首先，我们看下几个核心概念：<br>
`Store`：保存数据的地方，你可以把它看成一个容器，整个应用只能有一个`Store`。
`State`：`Store`对象包含所有数据，如果想得到某个时点的数据，就要对`Store`生成快照，这种时点的数据集合，就叫做`State`。
`Action`：`State`的变化，会导致`View`的变化。但是，用户接触不到`State`，只能接触到`View`。所以，`State`的变化必须是`View`导致的。`Action`就是`View`发出的通知，表示`State`应该要发生变化了。
`Action Creator`：`View`要发送多少种消息，就会有多少种`Action`。如果都手写，会很麻烦，所以我们定义一个函数来生成`Action`，这个函数就叫`Action Creator`。
`Reducer`：`Store`收到`Action`以后，必须给出一个新的`State`，这样`View`才会发生变化。这种`State`的计算过程就叫做`Reducer`。`Reducer`是一个函数，它接受`Action`和当前`State`作为参数，返回一个新的`State`。
`dispatch`：是`View`发出`Action`的唯一方法。

然后我们过下整个工作流程：

首先，用户（通过`View`）发出`Action`，发出方式就用到了`dispatch`方法。
然后，`Store`自动调用`Reducer`，并且传入两个参数：当前`State`和收到的`Action`，`Reducer`会返回新的`State`
`State`一旦有变化，`Store`就会调用监听函数，来更新`View`。

#### `react-redux`是如何工作的?
`Provider`: `Provider`的作用是从最外部封装了整个应用，并向`connect`模块传递`store`<br/>
`connect`: 负责连接`React`和`Redux`<br/>
- 获取`state`: `connect`通过`context`获取`Provider`中的`store`，通过`store.getState()`获取整个`store tree` 上所有`state`
- 包装原组件: 将`state`和`action`通过`props`的方式传入到原组件内部`wrapWithConnect`返回一个`ReactComponent`对象`Connect`，`Connect`重新`render`外部传入的原组件`WrappedComponent`，并把`connect`中传入的`mapStateToProps`, `mapDispatchToProps`与组件上原有的`props`合并后，通过属性的方式传给`WrappedComponent`
- 监听`store tree`变化: `connect`缓存了`store tree`中`state`的状态,通过当前`state`状态和变更前`state`状态进行比较,从而确定是否调用`this.setState()`方法触发`Connect`及其子组件的重新渲染

#### `redux`与`mobx`的区别?
两者对比:<br/>
- `redux`将数据保存在单一的`store`中，`mobx`将数据保存在分散的多个`store`中
- `redux`使用`plain object`保存数据，需要手动处理变化后的操作；`mobx`适用`observable`保存数据，数据变化后自动处理响应的操作
- `redux`使用不可变状态，这意味着状态是只读的，不能直接去修改它，而是应该返回一个新的状态，同时使用纯函数；`mobx`中的状态是可变的，可以直接对其进行修改
- `mobx`相对来说比较简单，在其中有很多的抽象，`mobx`更多的使用面向对象的编程思维；`redux`会比较复杂，因为其中的函数式编程思想掌握起来不是那么容易，同时需要借助一系列的中间件来处理异步和副作用
- `mobx`中有更多的抽象和封装，调试会比较困难，同时结果也难以预测；而r`edux`提供能够进行时间回溯的开发工具，同时其纯函数以及更少的抽象，让调试变得更加的容易

场景辨析:<br>
- 基于以上区别,我们可以简单得分析一下两者的不同使用场景.
- `mobx`更适合数据不复杂的应用: `mobx`难以调试,很多状态无法回溯,面对复杂度高的应用时,往往力不从心.
- `redux`适合有回溯需求的应用: 比如一个画板应用、一个表格应用，很多时候需要撤销、重做等操作，由于`redux`不可变的特性，天然支持这些操作.
- `mobx`适合短平快的项目: `mobx`上手简单,样板代码少,可以很大程度上提高开发效率.
- 当然`mobx`和`redux`也并不一定是非此即彼的关系,你也可以在项目中用`redux`作为全局状态管理,用`mobx`作为组件局部状态管理器来用.

#### `redux`中如何进行异步操作?
当然,我们可以在`componentDidmount`中直接进行请求无须借助`redux`.
但是在一定规模的项目中,上述方法很难进行异步流的管理,通常情况下我们会借助`redux`的异步中间件进行异步处理.
`redux`异步流中间件其实有很多,但是当下主流的异步中间件只有两种`redux-thunk`、`redux-saga`，当然`redux-observable`可能也有资格占据一席之地

####  `React` 中 `keys` 的作用是什么？
`Keys` 是 `React` 用于追踪哪些列表中元素被修改、被添加或者被移除的辅助标识。
- `react`利用`key`来识别组件，它是一种身份标识标识，相同的`key react`认为是同一个组件，这样后续相同的`key`对应组件都不会被创建
- 有了`key`属性后，就可以与组件建立了一种对应关系，`react`根据`key`来决定是销毁重新创建组件还是更新组件。
- `key`相同，若组件属性有所变化，则`react`只更新组件对应的属性；没有变化则不更新。
- `key`值不同，则`react`先销毁该组件(有状态组件的`componentWillUnmount`会执行)，然后重新创建该组件（有状态组件的`constructor`和`componentWillUnmount`都会执行）

#### 调用 `setState` 之后发生了什么？
- 在代码中调用 `setState` 函数之后，`React` 会将传入的参数对象与组件当前的状态合并，然后触发所谓的调和过程。
- 经过调和过程，`React`会以相对高效的方式根据新的状态构建`React`元素树并且着手重新渲染整个 `UI` 界面。
- 在 `React` 得到元素树之后，`React` 会自动计算出新的树与老树的节点差异，然后根据差异对界面进行最小化重渲染。
- 在差异计算算法中，`React` 能够相对精确地知道哪些位置发生了改变以及应该如何改变，这就保证了按需更新，而不是全部重新渲染。

#### 触发多次`setstate`，那么`render`会执行几次？
- 多次`setState`会合并为一次`render`，因为`setState`并不会立即改变`state`的值，而是将其放到一个任务队列里，最终将多个`setState`合并，一次性更新页面。
- 所以我们可以在代码里多次调用`setState`，每次只需要关注当前修改的字段即可。

#### 为什么建议传递给 `setState`的参数是一个`callback`而不是一个对象？
因为`this.props` 和`this.state`的更新可能是异步的，不能依赖它们的值去计算下一个`state`

#### 为什么`setState`是一个异步的？
当批量执行`state`的时候可以让`DOM`渲染的更快,也就是说多个`setstate`在执行的过程中还需要被合并

#### `this.setState`之后`react`做了哪些操作？
`shouldComponentUpdate`<br>
`componentWillUpdate`<br>
`render`<br>
`componentDidUpdate`

#### 简述一下`virtual DOM` （虚拟`dom`）如何工作？为什么虚拟 `dom` 会提高性能?
- 当数据发生变化，比如`setState`时，会引起组件重新渲染，整个`UI`都会以`virtual dom`的形式重新渲染
- 然后收集差异也就是`diff`新的`virtual dom`和老的`virtual dom`的差异
- 最后把差异队列里的差异，比如增加节点、删除节点、移动节点更新到真实的`DOM`上

#### 为什么虚拟 `dom` 会提高性能?
虚拟 `dom` 相当于在 `js` 和真实 `dom` 中间加了一个缓存，利用 `dom diff` 算法避免了没有必要的 `dom` 操作，从而提高性能。
用 `JavaScript` 对象结构表示 `DOM` 树的结构
然后用这个树构建一个真正的 `DOM` 树，插到文档当中当状态变更的时候，重新构造一棵新的对象树。
然后用新的树和旧的树进行比较，记录两棵树差异把 `2` 所记录的差异应用到步骤 `1` 所构建的真正的 `DOM` 树上，视图就更新了。

#### `react diff` 原理
- 把树形结构按照层级分解，只比较同级元素。
- 给列表结构的每个单元添加唯一的 `key` 属性，方便比较。
- `React` 只会匹配相同`class`的`component`（这里面的`class`指的是组件的名字）
- 合并操作，调用 `component` 的 `setState` 方法的时候, `React` 将其标记为 `dirty`到每一个事件循环结束, `React`检查所有标记 `dirty` 的 `component` 重新绘制.
- 选择性子树渲染。开发人员可以重写 `shouldComponentUpdate` 提高 `diff` 的性能。

#### `React` 中 `refs` 的作用是什么？
- `Refs` 是 `React` 提供给我们的安全访问`DOM`元素或者某个组件实例的句柄。
- 是父组件用来获取子组件的`dom`元素的
- 我们可以为元素添加 `ref` 属性然后在回调函数中接受该元素在 `DOM` 树中的句柄，该值会作为回调函数的第一个参数返回

#### 如果你创建了类似下面的 Twitter 元素，那么它相关的类定义是什么样的？
```jsx
<Twitter username='123'>
  {user => user === null 
  ? <Loading />
  : <Badge info={user}>
  }
</Twitter>
```
回调渲染模式：
```jsx
import React, { Component, PropTypes } from 'react'
import fetchUser from 'twitter'
class Twitter extends Component {
  state = {
    user: null,
  }
  static propTypes = {
    username: PropTypes.string.isRequired,
  }
  componentDidMount () {
    fetchUser(this.props.username)
      .then((user) => this.setState({user}))
  }
  render () {
    return this.props.children(this.state.user)
  }
}
```
这种模式的优势在于将父组件与子组件解耦和，父组件可以直接访问子组件的内部状态而不需要再通过 `Props` 传递，这样父组件能够更为方便地控制子组件展示的 `UI` 界面。

#### `Controlled Component` 与 `Uncontrolled Component` 之间的区别是什么？
`React` 的核心组成之一就是能够维持内部状态的自治组件，不过当我们引入原生的HTML表单元素时（`input,select,textarea` 等），我们是否应该将所有的数据托管到 `React` 组件中还是将其仍然保留在 DOM 元素中呢？这个问题的答案就是受控组件与非受控组件的定义分割。受控组件`（Controlled Component）`代指那些交由 `React` 控制并且所有的表单数据统一存放的组件。譬如下面这段代码中`username`变量值并没有存放到`DOM`元素中，而是存放在组件状态数据中。任何时候我们需要改变`username`变量值时，我们应当调用`setState`函数进行修改。
```jsx
class ControlledForm extends Component {
  state = {
    username: ''
  }
  updateUsername = (e) => {
    this.setState({
      username: e.target.value,
    })
  }
  handleSubmit = () => {}
  render () {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type='text'
          value={this.state.username}
          onChange={this.updateUsername} />
        <button type='submit'>Submit</button>
      </form>
    )
  }
}
```
而非受控组件`（Uncontrolled Component）`则是由`DOM`存放表单数据，并非存放在 `React` 组件中。我们可以使用 `refs` 来操控`DOM`元素
```jsx
class UnControlledForm extends Component {
  handleSubmit = () => {
    console.log("Input Value: ", this.input.value)
  }
  render () {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type='text'
          ref={(input) => this.input = input} />
        <button type='submit'>Submit</button>
      </form>
    )
  }
}
```
竟然非受控组件看上去更好实现，我们可以直接从 `DOM` 中抓取数据，而不需要添加额外的代码。不过实际开发中我们并不提倡使用非受控组件，因为实际情况下我们需要更多的考虑表单验证、选择性的开启或者关闭按钮点击、强制输入格式等功能支持，而此时我们将数据托管到 `React` 中有助于我们更好地以声明式的方式完成这些功能。引入 `React` 或者其他 `MVVM` 框架最初的原因就是为了将我们从繁重的直接操作 `DOM` 中解放出来。

#### 在生命周期中的哪一步你应该发起 `AJAX` 请求？
`React` 下一代调和算法 `Fiber` 会通过开始或停止渲染的方式优化应用性能，其会影响到 `componentWillMount` 的触发次数。对于 `componentWillMount` 这个生命周期函数的调用次数会变得不确定，`React` 可能会多次频繁调用 `componentWillMount`。如果我们将 `AJAX` 请求放到 `componentWillMount` 函数中，那么显而易见其会被触发多次，自然也就不是好的选择。<br>
如果我们将 `AJAX` 请求放置在生命周期的其他函数中，我们并不能保证请求仅在组件挂载完毕后才会要求响应。如果我们的数据请求在组件挂载之前就完成，并且调用了`setState`函数将数据添加到组件状态中，对于未挂载的组件则会报错。而在 `componentDidMount` 函数中进行 `AJAX` 请求则能有效避免这个问题。

#### `shouldComponentUpdate` 的作用是啥以及为何它这么重要？
`shouldComponentUpdate` 允许我们手动地判断是否要进行组件更新，根据组件的应用场景设置函数的合理返回值能够帮我们避免不必要的更新。

#### 如何告诉 `React` 它应该编译生产环境版本？
通常情况下我们会使用 `Webpack` 的 `DefinePlugin` 方法来将 `NODE_ENV` 变量值设置为 `production`。编译版本中 `React` 会忽略 `propType` 验证以及其他的告警信息，同时还会降低代码库的大小，`React` 使用了 `Uglify` 插件来移除生产环境下不必要的注释等信息。

#### 为什么我们需要使用 `React` 提供的 `Children` `API` 而不是 `JavaScript` 的 `map`？
`props.children`并不一定是数组类型，譬如下面这个元素：
```jsx
<Parent>
  <h1>Welcome.</h1>
</Parent>
```
如果我们使用`props.children.map`函数来遍历时会受到异常提示，因为在这种情况下`props.children`是对象`（object）`而不是数组`（array）`。`React` 当且仅当超过一个子元素的情况下会将`props.children`设置为数组，就像下面这个代码片：
```jsx
<Parent>
  <h1>Welcome.</h1>
  <h2>props.children will now be an array</h2>
</Parent>
```
这也就是我们优先选择使用`React.Children.map`函数的原因，其已经将`props.children`不同类型的情况考虑在内了。

#### 概述下 `React` 中的事件处理逻辑
为了解决跨浏览器兼容性问题，`React` 会将浏览器原生事件`（Browser Native Event）`封装为合成事件`（SyntheticEvent）`传入设置的事件处理器中。这里的合成事件提供了与原生事件相同的接口，不过它们屏蔽了底层浏览器的细节差异，保证了行为的一致性。另外有意思的是，`React` 并没有直接将事件附着到子元素上，而是以单一事件监听器的方式将所有的事件发送到顶层进行处理。这样 `React` 在更新 `DOM` 的时候就不需要考虑如何去处理附着在 `DOM` 上的事件监听器，最终达到优化性能的目的。

#### `createElement` 与 `cloneElement` 的区别是什么？
`createElement` 函数是 `JSX` 编译之后使用的创建 `React Element` 的函数，而 `cloneElement` 则是用于复制某个元素并传入新的 `Props`。

#### 传入 `setState` 函数的第二个参数的作用是什么？
该函数会在`setState`函数调用完成并且组件开始重渲染的时候被调用，我们可以用该函数来监听渲染是否完成。

#### react-router实现原理
react-router怎么实现页面局部刷新和url变化的?

路由的原理并不复杂，即保证视图和URL的同步，只要URL一致，那么返回的UI界面总是相同的。从性能和用户体验的层面来比较的话，后端路由每次访问一个新页面的时候都要向服务器发送请求，然后服务器再响应请求，这个过程肯定会有延迟。而前端路由在访问一个新页面的时候仅仅是变换了一下路径而已，没有了网络延迟，对于用户体验来说会有相当大的提升。

在HTML5的historyAPI出现之前，前端的路由都是通过hash来实现的，hash能兼容低版本的浏览器。Web服务并不会解析hash，也就是说#后的内容Web服务都会自动忽略，但是JavaScript是可以通过window.location.hash读取到的，读取到路径加以解析之后就可以响应不同路径的逻辑处理。

history是HTML5才有的新API，可以用来操作浏览器的session history(会话历史)。基于history来实现的路由可以和最初的路径规则一样，即不带#。用户可能都察觉不到该访问地址是Web服务实现的路由还是前端实现的路由。

react-router路由系统的核心是history对象，它的很多特性也与React保持了一致，比如声明式组件、组件嵌套、状态机特性等，毕竟它就是基于React构建并且为之所用的。路由进入时装载匹配的组件离开时卸载该组件的策略可以做到合理利用资源，不会一下把所有的组件都装载进来使内存占用飙升，也不会离开时没有卸载而时内存泄漏，每一个路由中声明的组件在渲染之前都会被传入一些props，主要包括history对象和location对象，这两个对象同时存在于路由组件的context中，还可以通过React的context API在组件的子级组件中获取到这两个对象。

点击Link后路由系统发生了什么？

Link组件最终会渲染为HTML的a标签，它的to、query、hash属性会被组合在一起并渲染为href属性。虽然Link被渲染为超链接，但在内部实现上使用脚本拦截了浏览器的默认行为，然后调用了history.pushState方法。history包中底层的pushState方法支持传入两个参数state和path，在函数体内又将这两个参数传输到createLocation方法中，返回location。系统会将这个location对象作为参数传入到TransitionTo方法中，然后调用window.location.hash或者window.history.pushState()修改了应用的URL，这取决于你创建history对象的方式。同时会触发history.listen中注册的事件监听器。

接下来请看路由系统内部是如何修改UI的。在得到了新的location对象后，系统内部的matchRoutes方法会匹配出Route组件树中与当前location对象匹配的一个子集，并且得到了nextState。在Router组件的componentWillMount生命周期方法中调用了history.listen(listener)方法。listener会在上述matchRoutes方法执行成功后执行listener(nextState)，nextState对象里面包含location、routes、params、components属性，接下来执行this.setState(nextState)就可以实现重新渲染Router组件。

点击浏览器的前进和后退按钮发生了什么？

前进与后退的实现，是通过监听popstate以及hashchange的事件，当前进或后退url更新时，触发这两个事件的回调函数。可以简单地把web浏览器的历史记录比做成一个仅有入栈操作的栈，当用户浏览器到某一个页面时将该文档存入到栈中，点击后退或前进按钮时移动指针到history栈中对应的某一个文档。在传统的浏览器中，文档都是从服务端请求过来的。不过现代的浏览器一般都会支持两种方式用于动态的生成并载入页面。

#### React Suspense

懒加载组件，利用动态 import 方式。
```js
import  React, { Component, Suspense } from "react"
const Test1 = React.lazy(()=>import("./Test1"))
const Test2 = React.lazy(()=>import("./Test1"))

function MyComponent(){
  return(
    <div>
      <Suspense fallback={<div>loading...</div>}> 
        <section>   // suspense可以包含多个需要懒加载的文件
            <Test1/> 
            <Test2/>
        </section>
      </Suspense>
    </div>
  )
} 
```

返回一个 Promise ，当 Promise 在pending状态时渲染 fallback 中的内容，resolve 之后渲染指定的内容。

**`Code Spliting`**<br/>
Code Spliting 在 React 中的使用方法是在 Suspense 组件中使用 <LazyComponent> 组件:
```jsx
import { Suspense, lazy } from 'react'

const DemoA = lazy(() => import('./demo/a'))
const DemoB = lazy(() => import('./demo/b'))

<Suspense>
  <NavLink to="/demoA">DemoA</NavLink>
  <NavLink to="/demoB">DemoB</NavLink>

  <Router>
    <DemoA path="/demoA" />
    <DemoB path="/demoB" />
  </Router>
</Suspense>
```
源码中 lazy 将传入的参数封装成一个 LazyComponent
```js
function lazy(ctor) {
  return {
    $$typeof: REACT_LAZY_TYPE, // 相关类型
    _ctor: ctor,
    _status: -1,   // dynamic import 的状态
    _result: null, // 存放加载文件的资源
  };
}
```
可以发现 dynamic import 本身类似 Promise 的执行机制, 也具有 Pending、Resolved、Rejected 三种状态, 这就比较好理解为什么 LazyComponent 组件需要放在 Suspense 中执行了(Suspense 中提供了相关的捕获机制)

**`Async Data Fetching`**
为了解决获取的数据在不同时刻进行展现的问题, Suspense 给出了解决方案。
一般进行数据获取的代码如下:
```jsx
export default class Demo extends Component {
  state = {
    data: null,
  };

  componentDidMount() {
    fetchAPI(`/api/demo/${this.props.id}`).then((data) => {
      this.setState({ data });
    });
  }

  render() {
    const { data } = this.state;

    if (data == null) {
      return <Spinner />;
    }

    const { name } = data;

    return (
      <div>{name}</div>
    );
  }
}
```
在 Suspense 中进行数据获取的代码如下:
```jsx
const resource = unstable_createResource((id) => {
  return fetchAPI(`/api/demo`)
})

function Demo {
  render() {
    const data = resource.read(this.props.id)

    const { name } = data;

    return (
      <div>{name}</div>
    );
  }
}
```
减少了 loading 状态的维护(在最外层的 Suspense 中统一维护子组件的 loading)<br/>
减少了不必要的生命周期的书写<br/>

具体原理过程：<br/>
1、第一次请求没有缓存, 子组件 throw 一个 thenable 对象, Suspense 组件内的 componentDidCatch 捕获之, 此时展示 Loading 组件;<br/>
2、当 Promise 态的对象变为完成态后, 页面刷新此时 resource.read() 获取到相应完成态的值;<br/>
3、之后如果相同参数的请求, 则走 LRU 缓存算法, 跳过 Loading 组件返回结果(缓存算法见后记);<br/>


#### React.lazy 懒加载实现原理
React.lazy() 所返回的 LazyComponent 对象，其 _status 默认是 -1，所以首次渲染时，会进入 readLazyComponentType 函数中的 default 的逻辑，这里才会真正异步执行 import(url)操作，由于并未等待，随后会检查模块是否 Resolved，如果已经Resolved了（已经加载完毕）则直接返回moduleObject.default（动态加载的模块的默认导出），否则将通过 throw 将 thenable 抛出到上层。<br/>

为什么要 throw 它？这就要涉及到 Suspense 的工作原理，我们接着往下分析。<br/>
Suspense 原理<br/>

由于 React 捕获异常并处理的代码逻辑比较多，这里就不贴源码，感兴趣可以去看 throwException 中的逻辑，其中就包含了如何处理捕获的异常。简单描述一下处理过程，React 捕获到异常之后，会判断异常是不是一个 thenable，如果是则会找到 SuspenseComponent ，如果 thenable 处于 pending 状态，则会将其 children 都渲染成 fallback 的值，一旦 thenable 被 resolve 则 SuspenseComponent 的子组件会重新渲染一次。

#### 怎么用react hooks模拟生命周期
```js
// constructor
unction Example() {
  // 在函数里初始化state
  const [count, setCount] = useState(0);
  return null;
}

// componentDidUpdate / componentDidMount
function Example() {
  // componentDidUpdate
  useEffect(() => console.log('mounted or updated'));
  // componentDidMount
  useEffect(() => console.log('mounted'), []);
  return null;
}
```
useEffect 拥有两个参数，第一个参数作为回调函数会在浏览器布局和绘制完成后调用，因此它不会阻碍浏览器的渲染进程。第二个参数是一个数组:

当数组存在并有值时，如果数组中的任何值发生更改，则每次渲染后都会触发回调。
当它不存在时，每次渲染后都会触发回调，类似于 componentDidUpdate。
当它是一个空列表时，回调只会被触发一次，类似于 componentDidMount。

```js
// shouldComponentUpdate
const MyComponent = React.memo(
    _MyComponent, 
    (prevProps, nextProps) => nextProps.count !== prevProps.count
)
```
React.memo 包裹一个组件来对它的 props 进行浅比较,但这不是一个 hooks，因为它的写法和 hooks 不同,其实React.memo 等效于 PureComponent，但它只比较 props。

|class 组件 |	Hooks 组件|
|--|
|constructor|	useState|
|getDerivedStateFromProps	|useState 里面 update 函数|
|shouldComponentUpdate|	useMemo|
|render|	函数本身|
|componentDidMount	|useEffect空数组或固定值|
|componentDidUpdate	|useEffect|
|componentWillUnmount	|useEffect 里面返回的函数|
|componentDidCatch	|无|
|getDerivedStateFromError	|无|

