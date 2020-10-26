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

