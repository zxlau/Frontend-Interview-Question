#### 基本数据类型
```js
undefined、null、Boolean、String、Number、Symbol
```
#### `JavaScript`原型、原型链?有什么特点？
每个`Function`对象都会在其内部初始化一个属性，就是`prototype`(原型)，当我们访问一个对象的属性时， 如果这个对象内部不存在这个属性，那么他就会去`prototype`里找这个属性，这个`prototype`又会有自己的`prototype`，于是就这样一直找下去，也就是我们平时所说的原型链的概念。<br/>
特点： `JavaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。
#### 原型链

<img width="500" src="/Frontend-Interview-Question/images/proto1.png" />
<img width="500" src="/Frontend-Interview-Question/images/proto2.png" />

```js
function Person() {}
let p = new Person();
// 原型链关系
Person.__proto__ === Function.prototype;
p.__proto__ === Person.prototype;

Person.prototype.__proto__ === Object.prototype
Object.prototype.__proto__ === null

class A {}
class B extends A {}
let a = new A();
let b = new B();
// 原型链关系
a.__proto__ === A.prototype;
b.__proto__ === B.prototype;
b.__proto__.__proto__ === A.prototype;

B.__proto__ === A;
B.prototype.__proto__ === A.prototype;

A.__proto__ === Function.prototype;
A.prototype.__proto__ === Object.prototype;

```
#### 如何实现数组的随机排序？
```js
let arr = [1,2,3,4,5,6,7,8,9,10]; 
function randSort1(arr) {
  for(let i = 0, len = arr.length; i < len; i++) {
    let rand = parseInt(Math.random() * len);
    [arr[rand], arr[i]] = [arr[i], arr[rand]];
  }
  return arr;
}
```
```js
// 著名的Fisher–Yates shuffle 洗牌算法
function shuffle(arr) {
  let m = arr.length;
  while(m > 1) {
    let index = parseInt(Math.random() * m--);
    [arr[index], arr[m]] = [arr[m], arr[index]]
  }
  return arr;
}
```
```js
let arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(() => {
  return Math.random() - 0.5;
})
```
#### 数组去重
```js
function removeDup(arr) {
  let result = [];
  let map = {};
  for(let i = 0; i < arr.length; i++) {
    let temp = arr[i];
    if(!map[temp]) {
      map[temp] = true;
      result.push(temp)
    }
  }
  return result;
}
// reduce
function removeDup(arr) {
  return arr.reduce((res, cur) => {
    let index = res.findIndex(i => i === cur);
    if(index == -1) {
      res.push(cur)
    }
    return res
  }, [])
}
```
```js
Array.from(new Set(arr))
```
```js
[...new Set(arr)]
```
#### `typeof`运算符和`instanceof`运算符以及`isPrototypeOf()`方法的区别
`typeof`是一个运算符，用于检测数据的类型，比如基本数据类型`undefined、string、number、boolean`，以及引用数据类型`object、function`，但是对于正则表达式、日期、数组这些引用数据类型，它会全部识别为`object`；<br/>
`instanceof`同样也是一个运算符，它就能很好识别数据具体是哪一种引用类型。它与`isPrototypeOf`的区别就是它是用来检测构造函数的原型是否存在于指定对象的原型链当中；而`isPrototypeOf`是用来检测调用此方法的对象是否存在于指定对象的原型链中，所以本质上就是检测目标不同。<br/>
#### 描述以下变量的区别：`null`、`undefined`或`undeclared`
`null` 表示"没有对象"，即该处不应该有值，转为数值时为0。<br/>
`undefined`表示"缺少值"，就是此处应该有一个值，但是还没有定义，转为数值时为`NaN`。<br/>
`undeclared`:`js`语法错误，没有申明直接使用，`js`无法找到对应的上下文。<br/>
#### `==`和`===`有什么区别？
首先，`== equality` 等同，`=== identity` 恒等。`==`，两边值类型不同的时候，要先进行类型转换，再比较。`===`，不做类型转换，类型不同的一定不等。

`===`比较
如果类型不同，就不相等
如果两个都是数值，并且是同一个值，那么相等；例外的是，如果其中至少一个是`NaN`，那么不相等。（判断一个值是否是`NaN`，只能用`isNaN()`来判断） 
如果两个都是字符串，每个位置的字符都一样，那么相等；否则[不相等]。 
如果两个值都是`true`，或者都是`false`，那么相等。 
如果两个值都引用同一个对象或函数，那么相等；否则不相等。 
如果两个值都是`null`，或者都是`undefined`，那么相等。 

`==`比较
如果一个是`null`、一个是`undefined`，那么[相等]。 
如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。 
如果任一值是 `true`，把它转换成 `1` 再比较；如果任一值是 `false`，把它转换成 `0` 再比较。 
如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的
`toString`或者`valueOf`方法。`js`核心内置类，会尝试`valueOf`先于`toString`；例外的是`Date`，`Date`利用的是`toString`转换。非`js`核心的对象
任何其他组合，都不相等。
#### 简述`js`中`this`的指向
第一准则是：`this`永远指向函数运行时所在的对象，而不是函数被创建时所在的对象。
普通的函数调用，函数被谁调用，`this`就是谁。<br/>
构造函数的话，如果不用new操作符而直接调用，那即`this`指向`window`。用`new`操作符生成对象实例后，`this`就指向了新生成的对象。<br/>
匿名函数或不处于任何对象中的函数指向`window`。<br/>
如果是`call，apply`等，指定的`this`是谁，就是谁。
#### 基本数据类型和引用数据类型
基本数据类型指的是简单的数据段，有`5`种，包括`null、undefined、string、boolean、number、symbol`；<br/>
引用数据类型指的是有多个值构成的对象，包括`object、array、date、regexp、function`等。<br/>
主要区别：<br/>
声明变量时不同的内存分配：前者由于占据的空间大小固定且较小，会被存储在栈当中，也就是变量访问的位置；后者则存储在堆当中，变量访问的其实是一个指针，它指向存储对象的内存地址。<br/>
也正是因为内存分配不同，在复制变量时也不一样。前者复制后2个变量是独立的，因为是把值拷贝了一份；后者则是复制了一个指针，2个变量指向的值是该指针所指向的内容，一旦一方修改，另一方也会受到影响。<br/>
参数传递不同：虽然函数的参数都是按值传递的，但是引用值传递的值是一个内存地址，实参和形参指向的是同一个对象，所以函数内部对这个参数的修改会体现在外部。原始值只是把变量里的值传递给参数，之后参数和这个变量互不影响。
#### 深拷贝和浅拷贝
如何区分深拷贝与浅拷贝，简单点来说，就是假设`B`复制了`A`，当修改`A`时，看`B`是否会发生变化，如果`B`也跟着变了，说明这是浅拷贝，拿人手短，如果B没变，那就是深拷贝，自食其力。
```js
function deepClone(obj) {
  let objClone = Array.isArray(obj) ? [] : {};
  if(obj && typeof obj === 'object') {
    for(let key in obj) {
      if(obj.hasOwnProperty(key)) {
        if(obj[key] && typeof obj[key] === 'object') {
          objClone[key] = deepClone(obj[key]);
        } else {
          objClone[key] = obj[key];
        }
      }
    }
  }
  return objClone;
}
```
```js
function deepClone(obj) {
  return JSON.parse(JSON.stringify(obj))
}
```
数组深拷贝方法：
```js
// ...
var a=[1,2,3]
var [...b]=a;//或b=[...a]
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3

// concat
var a=[1,2,3]
var c=[];
var b=c.concat(a);
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3

// slice
var a=[1,2,3]
var b=a.slice(0);
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3

// JSON
var a=[1,2,3]
var c=JSON.stringify(a);
var b=JSON.parse(c);
b.push(4);
console.log(b);//1,2,3,4
console.log(a)//1,2,3
```
#### `JSON.parse(JSON.stringify())`实现对对象的深拷贝的一些缺陷
1、如果`obj`里面有时间对象，则`JSON.stringify`后再`JSON.parse`的结果，时间将只是字符串的形式;<br>
2、如果`obj`里有`RegExp、Error`对象，则序列化的结果将只得到空对象;<br>
3、如果`obj`里有函数或者`undefined`，则序列化的结果会把函数或`undefined`丢失;<br>
4、如果`obj`里有`NaN、Infinity`和`-Infinity`，则序列化的结果会变成`null`;<br>
5、`JSON.stringify()`只能序列化对象的可枚举的自有属性，例如 如果`obj`中的对象是有构造函数生成的， 则使用`JSON.parse(JSON.stringify(obj))`深拷贝后，会丢弃对象的`constructor`;<br>
6、如果对象中存在循环引用的情况也无法正确实现深拷贝<br>

#### 原型继承的原理
**原型链**
```js
function BasicType() {
     this.property=true;
     this.getBasicValue = function(){
     return this.property;
      };
}
function NewType() {
     this.subproperty=false;
}
NewType.prototype = new BasicType(); 
var test = new NewType();
alert(test.getBasicValue());   //true
```
由上面可以知道，其本质上是重写了原型对象，代之一个新类型的实例。在通过原型链继承的情况下，要访问一个实例属性时，要经过三个步骤： 
1、搜索实例；<br/>
2、搜索`NewType.prototype`；<br/>
3、搜索`BasicType.prototype`,此时才找到方法。如果找不到属性或者方法，会一直向上回溯到末端才会停止。<br/>
要想确定实例和原型的关系，可以使用`instanceof`和`isPrototypeof()`测试，只要是原型链中出现过的原型，都可以说是该原型链所派生实例的原型。还有一点需要注意，通过原型链实现继承时，不能使用对象字面量创建原型方法，因为这时会重写原型链，原型链会被截断<br/>
**借用构造函数继承**
```js
function BasicType(name) {
     this.name=name;
     this.color=["red","blue","green"];
}
function NewType() {
     BasicType.call(this,"syf");
     this.age=23;
}
var test = new NewType(); 

alert(text.name); //syf
alert(text.age);   //23
```
**组合式继承**
```js
function BasicType(name) {
     this.name=name;
     this.colors=["red","blue","green"];
}
BasicType.prototype.sayName=function(){
     alert(this.name);
}
function NewType(name,age) {
     BasicType.call(this,name);
     this.age=age;
}
NewType.prototype = Object.create(BasicType.prototype);
var test = new NewType("syf","23");
test.colors.push("black");
alert(test.colors);  //"red,blue,green,black"
alert(test.name);   //"syf"
alert(test.age);   //23

// 组合式继承避免了原型链和借用构造函数继承方式的缺陷，融合了他们的优点，成为了js中最常用的继承方式。
```
#### `JavaScript`里`arguments`究竟是什么？
`Javascript`中每个函数都会有一个`Arguments`对象实例`arguments`，它引用着函数的实参，可以用数组下标的方式"[]"引用`arguments`的元素。`arguments.length`为函数实参个数，`arguments.callee`引用函数自身。<br/>
`arguments`虽然有一些数组的性质，但其并非真正的数组，只是一个类数组对象。其并没有数组的很多方法，不能像真正的数组那样调用`.jion(),.concat(),.pop()`等方法。
#### 基本类型转换
```js
// 请在问号处填写你的答案,使下方等式成立
let a = ?;
if(a == 1 && a == 2 && a == 3) {
  console.log("Hi, I'm Echi");
}
```
```js
let a = {
  i: 1,
  valueOf() {
    return this.i++;
  }
}
```
对象在转换基本类型时，会调用`valueOf`和`toString`，并且这两个方法你是可以重写的。<br/>
调用哪个方法，主要是要看这个对象倾向于转换为什么。如果倾向于转换为`Number`类型的，就优先调用`valueOf`;如果倾向于转换为`String`类型，就只调用`toString`。
```js
let obj = {
  toString () {
    console.log('toString')
    return 'string'
  },
  valueOf () {
    console.log('valueOf')
    return 'value'
  }
}

alert(obj) // string
console.log(1 + obj) // 1value
```
如果重写了`toString`方法，而没有重写`valueOf`方法，则会调用`toString`方法
```js
var obj = {
  toString () {
    return 'string'
  }
}
console.log(1 + obj) // 1string
```
调用上述两个方法的时候，需要 `return` 原始类型的值 `(primitive value)`，不能是`object、array`等非原始类型的值，不然的话就会去调用另外一个没有返回非原始类型的方法，如果都没有的话就会报错<br/>
如果有`Symbol.toPrimitive`属性的话，会优先调用，它的优先级最高<br/>
```js
let obj = {
  toString () {
    console.log('toString')
    return {}
  },
  valueOf () {
    console.log('valueOf')
    return {}
  },
  [Symbol.toPrimitive] () {
    console.log('primitive')
    return 'primi'
  }
}
console.log(1 + obj) // 1primi
```
#### 解析 `['1', '2, '3'].map(parseInt)`
```js
let new_array = arr.map(function callback(currentValue,index,array)
```
这个 `callback` 一共可以接收三个参数，其中第一个参数代表当前被处理的元素，而第二个参数代表该元素的索引。<br/>
而 `parseInt` 则是用来解析字符串的，使字符串成为指定基数的整数。 `parseInt(string, radix)`接收两个参数，第一个表示被处理的值（字符串），第二个表示为解析时的基数。<br/>
了解这两个函数后，我们可以模拟一下运行情况 `parseInt(‘1’, 0) //radix` 为 `0` 时，且 `string` 参数不以`“0x”`和`“0”`开头时，按照 `10` 为基数处理。这个时候返回 `1`；<br/>
`parseInt('2', 1)` // 基数为 `1`（`1` 进制）表示的数中，最大值小于 `2`，所以无法解析，返回 `NaN`；<br/>
`parseInt('3', 2)` // 基数为 `2`（`2` 进制）表示的数中，最大值小于 `3`，所以无法解析，返回 `NaN`。<br/>
`map` 函数返回的是一个数组，所以最后结果为 `[1, NaN, NaN]`。
#### 什么是防抖和节流？有什么区别？如何实现？
防抖：触发高频事件后 `n` 秒内函数只会执行一次，如果 `n` 秒内高频事件再次被触发，则重新计算时间。<br/>
思路：每次触发事件时都取消之前的延时调用方法
```js
function debounce(fn) {
  let timeout =  null;
  return function() {
    clearTimeout(timeout);
    timeout = setTimeout(() => {
      fn.apply(null, arguments)
    },500)
  }
}
```
示例： 
```js
function sayHi() { console.log('防抖成功'); } 
var inp = document.getElementById('inp'); 
inp.addEventListener('input', debounce(sayHi)); // 防抖
```
节流：高频事件触发，但在 `n` 秒内只会执行一次，所以节流会稀释函数的执行频率。<br/>
思路：每次触发事件时都判断当前是否有等待执行的延时函数<br/>
```js
function throttle(fn) {
  let canRun = true;
  return function() {
    if(!canRun) return;
    canRun = false;
    setTimeout(() => {
      fn.apply(null, arguments);
      canRun = true;
    }, 500)
  }
}
```
示例:
```js
function sayHi(e) {
  console.log(e.target.innerWidth, e.target.innerHeight);
} 
window.addEventListener('resize', throttle(sayHi));
```
#### 介绍下 `Set、Map、WeakSet` 和 `WeakMap` 的区别？
`Set`<br/>
成员唯一、无序且不重复；`[value, value]`，键值与键名是一致的（或者说只有键值，没有键名）；可以遍历，方法有：`add、delete、has`。<br/>
`WeakSet`<br/>
成员都是对象； 成员都是弱引用，可以被垃圾回收机制回收，可以用来保存 `DOM`节点，不容易造成内存泄漏； 不能遍历，方法有 `add、delete、has`。<br/>
`Map`<br/>
本质上是键值对的集合，类似集合； 可以遍历，方法很多，可以跟各种数据格式转换。<br/>
`WeakMap`<br/>
只接受对象为键名（`null` 除外），不接受其他类型的值作为键名； 键名是弱引用，键值可以是任意的，键名所指向的对象可以被垃圾回收，此时键名是无效的； 不能遍历，方法有 `get、set、has、delete`。<br/>
#### 介绍下深度优先遍历和广度优先遍历
**深度优先遍历`(DFS)`**<br/>
`DFS` 就是从图中的一个节点开始追溯，直到最后一个节点，然后回溯，继续追溯下一条路径，直到到达所有的节点，如此往复，直到没有路径为止。<br/>
```js
//深度优先遍历的递归写法
function deepTraversal(node){
  let nodes=[];
  if(node!=null){
      nodes.push[node];
      let childrens=node.children;
      for(let i=0;i<childrens.length;i++) {
        deepTraversal(childrens[i]);
      }
  }
  return nodes;
}
```
**广度优先遍历`(BFS)`**<br/>
`BFS`从一个节点开始，尝试访问尽可能靠近它的目标节点。本质上这种遍历在图上是逐层移动的，首先检查最靠近第一个节点的层，再逐渐向下移动到离起始节点最远的层。
```js
//广度优先遍历的递归写法
function wideTraversal(node){
    let nodes=[],i=0;
    if(node!=null){
        nodes.push(node);
        wideTraversal(node.nextElementSibling);
        node=nodes[i++];
        wideTraversal(node.firstElementChild);
    }
    return nodes;
}
```
#### 将数组扁平化并去除其中重复数据，最终得到一个升序且不重复的数组
```js
Array.from(new Set(arr.flat(Infinity))).sort((a,b)=>{ return a-b})
```
#### `JS`异步解决方案的发展历程以及优缺点
**1、回调函数`(callback)`**<br/>
缺点：回调地狱，不能用 `try catch` 捕获错误，不能`return`<br/>
回调地狱的根本问题在于：<br/>
缺乏顺序性： 回调地狱导致的调试困难，和大脑的思维方式不符； 嵌套函数存在耦合性，一旦有所改动，就会牵一发而动全身，即（控制反转）； 嵌套函数过多的多话，很难处理错误。
**2、`Promise`**<br>
`Promise` 就是为了解决 `callback` 的问题而产生的。<br>
`Promise` 实现了链式调用，也就是说每次`then`后返回的都是一个全新`Promise`，如果我们在`then`中`return`，`return`的结果会被`Promise.resolve()`包装。
优点：解决了回调地狱的问题。<br>
缺点：无法取消 Promise ，错误需要通过回调函数来捕获。<br>
**3、`Generator`**
特点：可以控制函数的执行，可以配合`co`函数库使用。
```js
function * fetch() {
    yield ajax('XXX1', () = >{}) 
    yield ajax('XXX2', () = >{}) 
    yield ajax('XXX3', () = >{})
}
let it = fetch() 
let result1 = it.next() 
let result2 = it.next() 
let result3 = it.next()
```
**4、`Async/await`**
`async、await` 是异步的终极解决方案。<br>
优点是：代码清晰，不用像 `Promise` 写一大堆 `then` 链，处理了回调地狱的问题；<br>
缺点：`await` 将异步代码改造成同步代码，如果多个异步操作没有依赖性而使用 `await` 会导致性能上的降低。
```js
let a = 0;
let b = async () = >{
    a = a + await 10;
    console.log('2', a); // -> '2' 10
}
b();
a++;
console.log('1', a); // -> '1' 1
```
首先函数`b`先执行，在执行到`await 10`之前变量`a`还是`0`，因为`await`内部实现了`generator` ，`generator`会保留堆栈中东西，所以这时候`a = 0`被保存了下来；<br>
因为`await`是异步操作，后来的表达式不返回`Promise`的话，就会包装成`Promise.reslove`(返回值)，然后会去执行函数外的同步代码；<br>
同步代码执行完毕后开始执行异步代码，将保存下来的值拿出来使用，这时候 `a = 0 + 10`。<br>
上述解释中提到了 `await` 内部实现了 `generator`，其实 `await` 就是 `generator` 加上 `Promise`的语法糖，且内部实现了自动执行 `generator`
#### `js`实现继承
```js
(function(){
  function Person() {
    this.name = 'person name'
  }
  Person.prototype.move = function() {
    console.log('person move')
  }
  function Man() {
    Person.call(this);
    this.name = 'man name'
  }
  Man.prototype = Object.create(Person.prototype)
  // 或者 Man.prototype = new Person()
  let man = new Man();
  man.name // man name
  man.move // person move
})()
```
#### `bind`实现
```js
Function.prototype.bind = function() {
  let self = this;
  let context = Array.prototype.shift.call(arguments);
  let args = Array.prototype.slice.call(arguments);
  return function() {
    let finalArgs = args.concat(Array.prototype.slice.call(arguments));
    self.apply(context, finalArgs)
  }
}
```
#### `filter`实现
```js
Array.prototype.filter = function(fn) {
  if(typeof fn !== 'Function') return;
  let arr = this;
  let result = [];
  for(let i = 0; i < arr.length; i++) {
    let temp = fn.call(null, arr[i], i, arr);
    if(temp) {
      result.push(arr[i]);
    }
  }
  return result;
}
```
#### `call`的实现
```js
Function.prototype.call = function(context) {
  let context = context || window;
  context.fn = this; //把调用call的方法作为指定的上下文对象的一个属性
  let args = [].silce.call(arguments, 1);
  let result = eval(`context.fn(${args})`);
  delete context.fn;
  return result;
}
```
#### `apply`的实现
```js
Function.prototype.apply = function(context) {
  let context = context || window;
  context.fn = this;
  let args = arguments[1];
  let result;
  if(args) {
    result = eval(`context.fn(${args})`)
  } else {
    result = context.fn()
  }
  delete context.fn;
  return result;
}
```
#### `instanceof`的实现
```js
function instanceOf(L, R) {
  let O = R.prototype;
  L = L.__proto__;
  while(true) {
    if(L === null) {
      return false;
    }
    if(O === L) {
      return true;
    }
    L = L.__proto__
  }
}
```
#### 宏任务/微任务执行顺序
```js
console.log('begin');
setTimeout(() => {
    console.log('setTimeout 1');
    Promise.resolve()
        .then(() => {
            console.log('promise 1');
            setTimeout(() => {
                console.log('setTimeout2');
            });
        })
        .then(() => {
            console.log('promise 2');
        });
    new Promise(resolve => {
        console.log('a');
        resolve();
    }).then(() => {
        console.log('b');
    });
}, 0);
console.log('end');

// begin -> end -> setTimeout 1 -> a -> promise1 -> b -> promise 2 -> setTimeout2
```
#### 执行顺序
```js
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})
// promise1,then11,promise2,then21,then12,then23
```
第一轮<br>
`current task: promise1`是当之无愧的立即执行的一个函数，立即执行输出`[promise1]`<br>
`micro task queue`: `[promise1的第一个then]`<br>

第二轮<br>
`current task`: `then1`执行中，立即输出了`then11`以及新`promise2`的`promise2`<br>
`micro task queue`: `[新promise2的then函数,以及promise1的第二个then函数]`<br>

第三轮<br>
`current task`: 新`promise2`的`then`函数输出`then21`和`promise1`的第二个`then`函数输出`then12`。<br>
`micro task queue`: `[新promise2的第二then函数]`<br>

第四轮<br>
`current task`: 新`promise2`的第二`then`函数输出`then23`<br>
`micro task queue: []`<br>
#### 执行顺序
```js
new Promise((resolve,reject)=>{
    console.log("promise1")
    resolve()
}).then(()=>{
    console.log("then11")
    return new Promise((resolve,reject)=>{
        console.log("promise2")
        resolve()
    }).then(()=>{
        console.log("then21")
    }).then(()=>{
        console.log("then23")
    })
}).then(()=>{
    console.log("then12")
})
// promise1,then11,promise2,then21,then23,then12
// 为何then12会在then23之后执行，这里Promise的第二个then相当于是挂在新Promise的最后一个then的返回值上
```
#### `this`指向
```js
let a = {
  b: function() { 
    console.log(this) 
  }, 
  c: () => {
    console.log(this)
  }
}
a.b()  // a对象
a.c() // window对象

let d = a.b;
d() // window对象
```
#### 补全代码
```js
function repeat(func, times, wait) {}
// 输入
const repeatFunc = repeat(alert, 4, 3000);
// 输出
// 会alert4次 helloworld, 每次间隔3秒
repeatFunc('hellworld');
// 会alert4次 worldhello, 每次间隔3秒
repeatFunc('worldhello')
```
```js
function repeat(func, times, s) {
  return async function(...args) {
    for(let i = 0; i < times; i++) {
      func.apply(null, args);
      await wait(s);
    }
  }
}
async function wait(seconds) {
  return new Promise((resolve,reject) => {
    setTimeout(resolve, seconds);
  })
} 
```
#### 如何在页面中改变另一个`iframe`的样式
```js
window.onload = function() {
  document.getElementById('frame的id').contentWindow.getElementById('frame里面的元素')
}
```
#### `require`和`import`的区别
**遵循规范**<br>
`require` 是 `AMD`规范引入方式<br>
`import`是`es6`的一个语法标准，如果要兼容浏览器的话必须转化成`es5`的语法

**调用时间**<br>
`require`是运行时调用，所以`require`理论上可以运用在代码的任何地方<br>
`import`是编译时调用，所以必须放在文件开头

**本质**<br>
`require`是赋值过程，其实`require`的结果就是对象、数字、字符串、函数等，再把`require`的结果赋值给某个变量<br>
`import`是解构过程，但是目前所有的引擎都还没有实现`import`，我们在`node`中使用`babel`支持`ES6`，也仅仅是将`ES6`转码为`ES5`再执行，`import`语法会被转码为`require`
#### `async await`
`async`函数就是`generator`函数的语法糖。`async`函数，就是将`generator`函数的`*`换成`async`，将`yield`替换成`await`。<br>【`async`函数对`generator`的改进】<br>
(1)内置执行器，不需要使用`next()`手动执行。<br>
(2)`await`命令后面可以是`Promise`对象或原始类型的值，`yield`命令后面只能是`Thunk`函数或`Promise`对象。<br>
(3)返回值是`Promise`。返回非`Promise`时，`async`函数会把它包装成`Promise`返回。`(Promise.resolve(value))`<br>
#### `Apply call`的区别
`call`和`apply`都是调用一个对象的一个方法，用另一个对象替换当前对象。而不同之处在于传递的参数，`apply`最多只能有两个参数——新`this`对象和一个数组`argArray`，如果`arg`不是数组则会报错`TypeError`。
#### 串行`promise`
```js
let list = [1, 2, 3, 4, 5];
let result = Promise.resolve();
list.forEach(item => {
  result = result.then(res => {
    return new Promise((resolve, reject) => {
      setTimeout(_ => {
        console.log(item)
        resolve(item)
      }, 500)
    })
  })
})
```
#### 串行`promise`,使用`async/await`
```js
let urls = [1, 2, 3, 4, 5];
let promises = [];
for(let i = 0; i < urls.length; i++) {
  promises.push(
    async function() {
      return new Promise((resolve, reject) => {
        setTimeout(_ => {
          console.log(i);
          resolve(i);
        }, 500)
      })
    }
  )
}

(async function(){
  for(let i = 0; i < promises.length; i++) {
    await promises[i]()
  }
})()
```
#### 实现一个`promiseAll`
```js
function promiseAll(promises) {
  if(!Array.isArray(promises)) return;
  return new Promise((resolve, reject) => {
    let result = [];
    let index = 0;
    for(let i = 0; i < promises.length; i++) {
      Promise.resolve(promises[i]).then(res => {
        result.push(res)
      })
      index++;
      if(index === promises.length) {
        resolve(result)
      }
    }
  })
}
```
#### 实现`promise.race`
```js
function promiseRace(promises) {
  if(!Array.isArray(promises)) return;
  return new Promise((resolve, reject) => {
    promises.forEach(p => {
      Promise.resolve(p).then(resolve, reject);
    })
  })
}
```
#### 实现一个`promise.retry`
`promise.retry` 的作用是执行一个函数，如果不成功最多可以尝试 `times` 次。传参需要三个变量，所要执行的函数，尝试的次数以及延迟的时间。
```js
Promise.prototype.retry = function(fn, times, delay) {
  return new Promise((resolve, reject) => {
    let error;
    let attempt = function() {
      if(times === 0) {
        reject(error)
      } else {
        fn.then(resolve)
          .catch(e => {
            times--;
            error = e;
            setTimeout(() => {
              attempt();
            }, delay)
          }) 
      }
    }
    attempt();
  })
}
```
#### 输出结果
```js
let inner = 'window';

function say() {
	console.log(inner);
	console.log(this.inner);
}

let obj1 = (function(){
	let inner = '1-1';
	return {
		inner: '1-2',
		say: function() {
			console.log(inner);
			console.log(this.inner)
		}
  }
})();
let obj2 = (function(){
	let inner = '2-1';
	return {
		inner: '2-2',
		say: function() {
			console.log(inner);
			console.log(this.inner)
		}
  }
})();
say(); // window    undefined 因为let 不会变量提升
obj1.say() // 1-1 闭包   1-2 this指向

obj1.say = say;
obj1.say()  // window 闭包 1-2 this指向

obj1.say = obj2.say;
obj1.say()  // 2-1 闭包 1-2 this指向
```
#### `Promise` 构造函数是同步执行还是异步执行，那么 `then` 方法呢？
```js
const promise = new Promise((resolve, reject) => {
  console.log(1)
  resolve()
  console.log(2)
})
romise.then(() => {
  console.log(3)
})

console.log(4)

// 执行结果是 1243
// 表明promise构造函数是同步执行的，then方法是异步执行的
```
#### 数组中增加一个方法 findDuplicate(n) 数组元素出现频率大于n，返回这些元素组成的数组
```js
Array.prototype.findDuplicate = function(count) {
  let result = this.reduce((res, item) => {
    let index = res.findIndex(i => i.name == item);
    if(index > 0) {
      res[index].count++;
    } else {
      res.push({name: item, count: 1})
    }
    return res;
  }, []);
  return result.filter(item => item.count > count).map(item => item.name);
}
```
#### 实现一个`promise`
```js
function Promise(fn) {
  let status = 'pending';
  let successArray = [];
  let errorArray = [];
  function successNotify() {
    status = 'resolve';
    handleThen.apply(undefined, arguments);
  }
  function errorNotify() {
    status = 'reject';
    handleThen.apply(undefined, arguments);
  }
  function handleThen() {
    setTimeout(_ => {
      if(status == 'resolve') {
        for(let i = 0 ;i < successArray.length; i++) {
          successArray[i].apply(undefined, arguments);
        }
      } else if(status == 'reject') {
        for(let i = 0 ;i < successArray.length; i++) {
          errorArray[i].apply(undefined, arguments);
        }
      }
    }, 0)
  }
  fn.call(undefined, successNotify, errorNotify);
  return {
    then: (successFn, errorFn) => {
      successArray.push(successFn);
      errorArray.push(errorFn);
      return undefined;
    }
  }
}
```
```js
// 更严谨
class Promise {
  static pending = 'pending';
  static fulfilled = 'fulfilled';
  static rejected = 'rejected';
  constructor(executor) {
    this.status = Promise.pending; //初始化状态为pending
    this.value = null; // 存储this._resolve操作成功返回的值
    this.reason = null; // 存储this._reject操作失败返回的值
    this.callbacks = [];
    executor(this._resolve.bind(this), this._reject.bind(this));
  }
  
  // onFulfilled 成功时执行
  // onRejected 失败时执行
  then(onFulfilled, onRejected) {
    // 返回新的 Promise
    return new Promise((nextResolve, nextReject) => {
      // 之所以把下一个Promise的resolve函数和reject函数也存在callback中
      // 是为了将onFullfilled的执行结果通过nextResolve传入到下一个Promise作为它的value值
      this._handler({
        nextResolve,
        nextReject,
        onFulfilled,
        onRejected
      })
    })
  }
  
  _resolve(value) {
    // 处理onFulfilled执行结果是一个Promise时的情况
    // 当value instanceOf Promise时，说明当前 Promise 肯定不会是第一个Promise
    // 而是后续then方法返回的Promise
    // 我们要获取的是value中的value(value是Promise时，那么内部存有个value的变量)
    // 怎样将value的value值取到呢，可以将传递一个函数作为value.then的onFulfilled参数
    // 那么在value的内部则会执行这个函数，我们只需要将当前Promise的value值赋值为value的value即可
    if(value instanceOf Promsie) {
      value.then(
        this._resolve.bind(this),
        this._reject.bind(this)
      );
      return
    }
    this.value = value;
    this.status = Promise.fulfilled;  // 设置状态为成功
    
    this.callbacks.forEach(cb => this.handler(cb)); // 通知事件执行
  }
  
  _reject(reason) {
    if(reason instanceOf Promsie) {
      reason.then(
        this._resolve.bind(this),
        this._reject.bind(this)
      );
      return
    }
    this.reason = reason;
    this.status = Promise.rejected;  // 设置状态为失败
    
    this.callbacks.forEach(cb => this.handler(cb)); // 通知事件执行
  }
  
  _handler(callback) {
    const {
      onFulfilled,
      onRejected,
      nextResolved,
      nextReject,
    } = callback;
    if(this.status === Promise.pending) {
      this.callbacks.push(callback);
      return;
    }
    
    if(this.status === Promise.fulfilled) {
      //传入存储的值
      // 未传入onfulfilled时,value传入
      const nextValue = onFulfilled ? onFulfilled(this.value) : this.value;
      nextResolved(nextValue);
      return;
    }
    
    if(this.status === Promise.rejected) {
      const nextReason = onReject ? onReject(this.reason) : this.reason;
      nextReject(nextReason);
    }
  }
}
```

#### `ES5/ES6`的继承除了写法以外还有什么区别？
```js
class Foo {}
```
1、`class`声明会提升，但不会初始化值,`Foo`进入暂时性死区，类似`let`和`const`的特点；
2、`class`声明内部会启动严格模式；
3、`class`中的所有方法都是不可枚举的；
4、`class`中的所有方法没有原型对象，不能使用`new`关键字来调用；
5、`class`必须使用`new`关键字调用
6、`class`内部的方法不能重写
#### `setTimeout、Promise、Async/Await` 的区别
这题主要是考察这三者在事件循环中的区别，事件循环中分为宏任务队列和微任务队列。<br>
其中`setTimeout`的回调函数放到宏任务队列里，等到执行栈清空以后执行；<br>
`promise.then`里的回调函数会放到相应宏任务的微任务队列里，等宏任务里面的同步代码执行完再执行；<br>
`async`函数表示函数里面可能会有异步方法，`await`后面跟一个表达式，`async`方法执行时，遇到`await`会立即执行表达式，然后把表达式后面的代码放到微任务队列里，让出执行栈让同步代码先执行。
#### 执行顺序
```js
async function async1() {
    console.log('async1 start');
    await async2();
    console.log('async1 end');
}
async function async2() {
    console.log('async2');
}
console.log('script start');
setTimeout(function() {
    console.log('setTimeout');
}, 0)
async1();
new Promise(function(resolve) {
    console.log('promise1');
    resolve();
}).then(function() {
    console.log('promise2');
});
console.log('script end');

// script start -> async1 start -> async2 -> promise1 -> script end -> async1 end  -> promise 2 -> setTimeout
// await async2() 这句中，遇到await会立即执行async2();
// 然后把await后面的语句 console.log('async1 end')加入到微任务中，
//所以等同步代码输出promise1 和 script end才会输出async1 end
```
#### 通过`new`创建一个对象的时候，构造函数内部有哪些改变？
创建一个空对象，将它的引用赋给 `this`，继承函数的原型。
通过 `this` 将属性和方法添加至这个对象
最后返回 `this` 指向的新对象，也就是实例
#### 实现一个`new`
```js
function _new(parent, ...args) {
   // 1.以构造器的prototype属性为原型，创建新对象；
  let child = Object.create(parent.prototype);
  // 2.将this和调用参数传给构造器执行
  parent.apply(child, args);
  // 3.返回第一步的对象
  return child;
}
```
#### 前端中的模块化开发发展历史
模块化主要是用来抽离公共代码，隔离作用域，避免变量冲突等。

`IIFE`： 使用自执行函数来编写模块化。特点：在一个单独的函数作用域中执行代码，避免变量冲突。<br>
`AMD`： 使用`requireJS` 来编写模块化，特点：依赖必须提前声明好。<br>
`CMD`： 使用`seaJS`来编写模块化，特点：支持动态引入依赖文件。<br>
`CommonJS`： `nodejs`中自带的模块化。<br>
`UMD`：兼容`AMD`，`CommonJS` 模块化语法。<br>
`webpack(require.ensure)`：`webpack 2.x` 版本中的代码分割。<br>
`ES Modules`： `ES6`引入的模块化，支持`import`来引入另一个`js` 。
#### export default与export的区别
`export`可以导出多个对象，`export default`只能导出一个对象；<br>
`export` 导出对象需要用`{}`，`export default`不需要`{}`，如：<br>
```js
export { A, B, C};
export default A;
```
在其他文件引用`export default`导出的对象时不一定使用导出时的名字。因为这种方式实际上是将该导出对象设置为默认导出对象，如：
```js
// 文件A
export default deObject;
// 文件B
import deObject from './A'
// 或者
import newDeObject from './A'
```

#### 改造下面的代码，使之输出`0 - 9`，写出你能想到的所有解法
```js
for (var i = 0; i< 10; i++){
	setTimeout(() => {
		console.log(i);
  }, 1000)
}
```
```js
// 利用 setTimeout 函数的第三个参数，会作为回调函数的第一个参数传入
for (var i = 0; i< 10; i++){
	setTimeout((i) => {
		console.log(i);
  }, 1000， i)
}
```
```js
// 利用 bind 函数部分执行的特性
for (var i = 0; i< 10; i++){
	setTimeout(console.log.bind(null, i), 1000);
}
```
```js
// let
for (let i = 0; i< 10; i++){
	setTimeout(() => {
		console.log(i);
  }, 1000)
}
```
```js
for (var i = 0; i< 10; i++){
  (i => {
    setTimeout(() => {
      console.log(i);
    }, 1000)
  })(i)
}
```
```js
// 利用 eval 或者 new Function 执行字符串
for (var i = 0; i < 10; i++) {
  setTimeout(eval('console.log(i)'), 1000)
}
for (var i = 0; i < 10; i++) {
  setTimeout(new Function('i', 'console.log(i)')(i), 1000)
}
for (var i = 0; i < 10; i++) {
  setTimeout(new Function('console.log(i)')(), 1000)
}
```
#### 下面的代码打印什么内容，为什么？
```js
var b = 10;
(function b(){
    b = 20;
    console.log(b); 
})();
```
函数名`b`只在该函数内部有效，并且此绑定是常量绑定。<br>
对于一个常量进行赋值，在 `strict` 模式下会报错，非 `strict` 模式下静默失败。所以函数内部`b=20`应该是无效的。<br>
`b`函数是一个相当于用`const`定义的常量，内部无法进行重新赋值，如果在严格模式下，会报错`"Uncaught TypeError: Assignment to constant variable."`
#### 输出
```js
var a = 10;
(function () {
    console.log(a)
    a = 5
    console.log(window.a)
    var a = 20;
    console.log(a)
})()
// undefined 10 20
```
在立即执行函数中，`var a = 20`; 语句定义了一个局部变量 `a`，由于`js`的变量声明提升机制，局部变量`a`的声明会被提升至立即执行函数的函数体最上方，且由于这样的提升并不包括赋值，因此第一条打印语句会打印`undefined`，最后一条语句会打印`20`。<br>

由于变量声明提升，`a = 5`; 这条语句执行时，局部的变量`a`已经声明，因此它产生的效果是对局部的变量`a`赋值，此时`window.a` 依旧是最开始赋值的`10`。
#### 实现一个 `sleep` 函数
比如 `sleep(1000)` 意味着等待`1000`毫秒，可从 `Promise、Generator、Async/Await` 等角度实现
```js
function wait(time) {
  return new Promise((resolve, reject) => {
    setTimeout(resolve, time);
  })
}
async function sleep(time) {
  await wait(time);
}
```
```js
function sleep(time) {
  function *gen() {
    yield new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve()
      }, time)
    })
  }
  return gen.next().value;
}
sleep(1000).then(_ => {
  console.log('sleep end')
})
```
#### 输出以下代码执行的结果并解释为什么
```js
var obj = {
    '2': 3,
    '3': 4,
    'length': 2,
    'splice': Array.prototype.splice,
    'push': Array.prototype.push
}
obj.push(1)
obj.push(2)
console.log(obj)
// [,,1,2], length为4
// 伪数组（ArrayLike）
```
这个`obj`中定义了两个`key`值，分别为`splice`和`push`分别对应数组原型中的`splice`和`push`方法，因此这个`obj`可以调用数组中的`push`和`splice`方法，调用对象的`push`方法：`push(1)`，因为此时`obj`中定义`length`为`2`，所以从数组中的第二项开始插入，也就是数组的第三项（下表为`2`的那一项），因为数组是从第`0`项开始的，这时已经定义了下标为`2`和`3`这两项，所以它会替换第三项也就是下标为`2`的值，第一次执行`push`完，此时`key`为`2`的属性值为`1`，同理：第二次执行`push`方法，`key`为`3`的属性值为`2`。此时的输出结果就是：
```js
Object(4) [empty × 2, 1, 2, splice: ƒ, push: ƒ]---->
[
2: 1,
3: 2,
length: 4,
push: ƒ push(),
splice: ƒ splice()
]
```
#### `call` 和 `apply` 的区别是什么，哪个性能更好一些
第一个参数都是，指定函数体内`this`的指向；<br>
第二个参数开始不同，`apply`是传入带下标的集合，数组或者类数组，`apply`把它传给函数作为参数，`call`从第二个开始传入的参数是不固定的，都会传给函数作为参数。<br>
`call`比`apply`的性能要好，平常可以多用`call`, `call`传入参数的格式正是内部所需要的格式,`apply`比`call`多了一个结构第二个参数的过程
#### 为什么通常在发送数据埋点请求的时候使用的是 `1x1` 像素的透明 `gif` 图片？
没有跨域问题，一般这种上报数据，代码要写通用的；（排除`ajax`）<br>
不会阻塞页面加载，影响用户的体验，只要`new Image`对象就好了<br>
在所有图片中，体积最小。
#### 实现 `(5).add(3).minus(2)` 功能
```js
Number.prototype.add = function(n) {
  return this + n;
}
Number.prototype.minus = function(n) {
  return this - n;
}
```
#### 输出结果，为什么
```js
var a = {n: 1};
var b = a;
a.x = a = {n: 2};

console.log(a.x) 	
console.log(b.x)
```
结果:<br>
`undefined`<br>
`{n:2}`<br>

首先，`a`和`b`同时引用了`{n:2}`对象，接着执行到`a.x = a = {n：2}`语句，尽管赋值是从右到左的没错，但是`.`的优先级比`=`要高，所以这里首先执行`a.x`，相当于为`a`（或者`b`）所指向的`{n:1}`对象新增了一个属性`x`，即此时对象将变为`{n:1;x:undefined}`。之后按正常情况，从右到左进行赋值，此时执行`a ={n:2}`的时候，`a`的引用改变，指向了新对象`{n：2}`,而`b`依然指向的是旧对象。之后执行`a.x = {n：2}`的时候，并不会重新解析一遍`a`，而是沿用最初解析`a.x`时候的`a`，也即旧对象，故此时旧对象的`x`的值为`{n：2}`，旧对象为 `{n:1;x:{n：2}}`，它被`b`引用着。 后面输出`a.x`的时候，又要解析`a`了，此时的`a`是指向新对象的`a`，而这个新对象是没有`x`属性的，故访问时输出`undefined`；而访问`b.x`的时候，将输出旧对象的`x`的值，即`{n:2}`。
#### 某公司 `1` 到 `12` 月份的销售额存在一个对象里面
如下：`{1:222, 2:123, 5:888}`，请把数据处理为如下结构：`[222, 123, null, null, 888, null, null, null, null, null, null, null]`。
```js
let obj = {1: 222, 2: 123, 5: 888};
let result = Array.from({length: 12}).map((item, index) => {
  return obj[index+1] || null
})
```
#### 箭头函数与普通函数（`function`）的区别是什么？构造函数（`function`）可以使用 `new` 生成实例，那么箭头函数可以吗？为什么？
1、函数体内的 `this` 对象，就是定义时所在的对象，而不是使用时所在的对象。<br>
2、不可以使用 `arguments` 对象，该对象在函数体内不存在。如果要用，可以用 `rest` 参数代替。<br>
3、不可以使用 `yield` 命令，因此箭头函数不能用作 `Generator` 函数。<br>
4、不可以使用 `new` 命令，因为：没有自己的 `this`，无法调用 `call，apply`。 没有 `prototype` 属性 ，而 `new` 命令在执行时需要将构造函数的 `prototype` 赋值给新的对象的 `__proto__`
#### 给定两个数组，写一个方法来计算它们的交集。
例如：给定 `nums1 = [1, 2, 2, 1]，nums2 = [2, 2]`，返回 `[2, 2]`。
```js
function intersection(arr1, arr2) {
  let result = [];
  let map = arr1.reduce((res, item) => {
    if(res[item]) {
      res[item]++;
    } else {
      res[item] = 1;
    }
    return res
  }, {});
  arr2.forEach(item => {
    if(map[item] &&map[item] > 0) {
      resutlt.push(item);
      map[item]--;
    }
  })
}
```
#### 介绍下如何实现 `token` 加密
需要一个`secret`随机数<br>
后端根据`secret` 和加密算法生成一个字符串，返回给前端<br>
前端每次`request`都在`header`中带上`token`<br>
后端用同样的算法解密

#### 实现一个`promise.finally`
```js
Promise.prototype.finally = function(callback) {
  return Promise.then(
    value => Promise.resolve(callback()).then(() => value),
    error => Promise.resolve(callback()).then(() => {throw error;})
  )
}
```
#### `a.b.c.d` 和 `a['b']['c']['d']`，哪个性能更高？
```js
应该是 `a.b.c.d` 比 `a['b']['c']['d']` 性能高点，后者还要考虑 `[ ]` 中是变量的情况，再者，从两种形式的结构来看，显然编译器解析前者要比后者容易些，自然也就快一点。
```
#### `ES6` 代码转成 `ES5`代码的实现思路是什么
解析：解析代码字符串，生成 `AST`<br>
转换： 按一定的规则转换、修改`AST`<br>
生成：将修改后的`AST`转换成普通代码
#### 使用 `JavaScript Proxy` 实现简单的数据绑定
```html
<body>
  hello,world
  <input type="text" id="model">
  <p id="word"></p>
</body>
```
```js
 const model = document.getElementById("model")
  const word = document.getElementById("word")
  var obj= {};

  const newObj = new Proxy(obj, {
      get: function(target, key, receiver) {
        console.log(`getting ${key}!`);
        return Reflect.get(target, key, receiver);
      },
      set: function(target, key, value, receiver) {
        console.log('setting',target, key, value, receiver);
        if (key === "text") {
          model.value = value;
          word.innerHTML = value;
        }
        return Reflect.set(target, key, value, receiver);
      }
    });

  model.addEventListener("keyup",function(e){
    newObj.text = e.target.value
  })
```
#### 数组里面有`10`万个数据，取第一个元素和第`10`万个元素的时间相差多少
数组可以直接根据索引取的对应的元素，所以不管取哪个位置的元素的时间复杂度都是 `O(1)`<br>
得出结论：消耗时间几乎一致，差异可以忽略不计
#### 输出以下代码运行结果
```js
// example 1
var a={}, b='123', c=123;  
a[b]='b';
a[c]='c';  
console.log(a[b]);

// example 2
var a={}, b=Symbol('123'), c=Symbol('123');  
a[b]='b';
a[c]='c';  
console.log(a[b]);

// example 3
var a={}, b={key:'123'}, c={key:'456'};  
a[b]='b';
a[c]='c';  
console.log(a[b]);

// example1
// c 的键名会被转换成字符串'123'，这里会把 b 覆盖掉。所以输出 c
// example2
// c 是 Symbol 类型，不需要转换。任何一个 Symbol 类型的值都是不相等的，所以不会覆盖掉 b。所以输出 b
// example3
// b 不是字符串也不是 Symbol 类型，需要转换成字符串。对象类型会调用 toString 方法转换成字符串 [object Object]。
// c 不是字符串也不是 Symbol 类型，需要转换成字符串。对象类型会调用 toString 方法转换成字符串 [object Object]。所以会覆盖。所以输出 c
```
#### 介绍下 `Promise.all` 使用、原理实现及错误处理
`Promise.all()`方法将多个`Promise`实例包装成一个`Promise`对象`（p）`，接受一个数组`（p1,p2,p3）`作为参数，数组中不一定需要都是`Promise`对象，但是一定具有`Iterator`接口，如果不是的话，就会调用`Promise.resolve`将其转化为`Promise`对象之后再进行处理。<br>

使用`Promise.all()`生成的`Promise`对象`（p）`的状态是由数组中的`Promise`对象`（p1,p2,p3）`决定的；<br>
1、如果所有的`Promise`对象`（p1,p2,p3）`都变成`fullfilled`状态的话，生成的`Promise`对象`（p）`也会变成`fullfilled`状态，`p1,p2,p3`三个`Promise`对象产生的结果会组成一个数组返回给传递给`p`的回调函数；<br>
2、如果`p1,p2,p3`中有一个`Promise`对象变为`rejected`状态的话，`p`也会变成`rejected`状态，第一个被`rejected`的对象的返回值会传递给`p`的回调函数。

`Promise.all()`方法生成的`Promise`对象也会有一个`catch`方法来捕获错误处理，但是如果数组中的`Promise`对象变成`rejected`状态时，并且这个对象还定义了`catch`的方法，那么`rejected`的对象会执行自己的`catch`方法，并且返回一个状态为`fullfilled`的`Promise`对象，`Promise.all()`生成的对象会接受这个`Promise`对象，不会返回`rejected`状态。
#### `var let const` 实现原理
`var`的话会直接在栈内存里预分配内存空间，然后等到实际语句执行的时候，再存储对应的变量，如果传的是引用类型，那么会在堆内存里开辟一个内存空间存储实际内容，栈内存会存储一个指向堆内存的指针。

`let`的话，是不会在栈内存里预分配内存空间，而且在栈内存分配变量时，做一个检查，如果已经有相同变量名存在就会报错。

`const`的话，也不会预分配内存空间，在栈内存分配变量时也会做同样的检查。不过`const`存储的变量是不可修改的，对于基本类型来说你无法修改定义的值，对于引用类型来说你无法修改栈内存里分配的指针，但是你可以修改指针指向的对象里面的属性。
#### 实现模糊搜索结果的关键词高亮显示
```js
// 考虑节流、缓存。其实还可以上列表diff+定时清理缓存,正则替换掉关键词
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>auto complete</title>
  <style>
    bdi {
      color: rgb(0, 136, 255);
    }

    li {
      list-style: none;
    }
  </style>
</head>
<body>
  <input class="inp" type="text">
  <section>
    <ul class="container"></ul>
  </section>
</body>
<script>
  function debounce(fn, timeout = 300) {
    let t;
    return (...args) => {
      if (t) {
        clearTimeout(t);
      }
      t = setTimeout(() => {
        fn.apply(fn, args);
      }, timeout);
    }
  }

  function memorize(fn) {
    const cache = new Map();
    return (name) => {
      if (!name) {
        container.innerHTML = '';
        return;
      }
      if (cache.get(name)) {
        container.innerHTML = cache.get(name);
        return;
      }
      const res = fn.call(fn, name).join('');
      cache.set(name, res);
      container.innerHTML = res;
    }
  }

  function handleInput(value) {
    const reg = new RegExp(`\(${value}\)`);
    const search = data.reduce((res, cur) => {
      if (reg.test(cur)) {
        const match = RegExp.$1;
        res.push(`<li>${cur.replace(match, '<bdi>$&</bdi>')}</li>`);
      }
      return res;
    }, []);
    return search;
  }
  
  const data = ["上海野生动物园", "上饶野生动物园", "北京巷子", "上海中心", "上海黄埔江", "迪士尼上海", "陆家嘴上海中心"]
  const container = document.querySelector('.container');
  const memorizeInput = memorize(handleInput);
  document.querySelector('.inp').addEventListener('input', debounce(e => {
    memorizeInput(e.target.value);
  }))
</script>
</html>
```
#### 代码打印结果
```js
// 感觉这么说不准确。对象传值传的是引用，但是引用是copy给函数形参。
// webSite引用地址的值copy给a了
function changeObjProperty(a) {
  // 改变对应地址内的对象属性值
  a.siteUrl = "http://www.baidu.com"
  // 变量a指向新的地址 以后的变动和旧地址无关
  a = new Object()
  a.siteUrl = "http://www.google.com"
  a.name = 456
} 
var webSite = new Object();
webSite.name = '123'
changeObjProperty(webSite);
console.log(webSite); // {name: 123, siteUrl: 'http://www.baidu.com'}
```
#### 代码输出结果
```js
function Foo() {
    Foo.a = function() {
        console.log(1)
    }
    this.a = function() {
        console.log(2)
    }
}
Foo.prototype.a = function() {
    console.log(3)
}
Foo.a = function() {
    console.log(4)
}
Foo.a(); 
// 立刻执行了 Foo 上的 a 方法，也就是刚刚定义的，所以
// # 输出 4
let obj = new Foo();
/* 这里调用了 Foo 的构建方法。Foo 的构建方法主要做了两件事：
1. 将全局的 Foo 上的直接方法 a 替换为一个输出 1 的方法。
2. 在新对象上挂载直接方法 a ，输出值为 2。
*/
obj.a();
// 因为有直接方法 a ，不需要去访问原型链，所以使用的是构建方法里所定义的 this.a，
// # 输出 2
Foo.a();
// 构建方法里已经替换了全局 Foo 上的 a 方法，所以
// # 输出 1
```
#### 函数中的`arguments`是数组吗？怎么转数组？
```js
Array.from(arguments)
[...arguments]
```
#### 以下打印结果
```js
if([]==false){console.log(1)};   // true []和false 都被转换成 0
if({}==false){console.log(2)};   // false {}被转换成 NAN, false被转化成 0
if([]){console.log(3)}           // true []被转换成true
if([1]==[1]){console.log(4)}     // false 两者是不同对象，地址不同
```
#### 获取页面元素的位置和宽高？
```js
element.clientWidth = content + padding
element.clientHeight = content + padding
```
#### `requestAnimationFrame` 原理？是同步还是异步？
异步。

`window.requestAnimationFrame()` 告诉浏览器——你希望执行一个动画，并且要求浏览器在下次重绘之前调用指定的回调函数更新动画。该方法需要传入一个回调函数作为参数，该回调函数会在浏览器下一次重绘之前执行。<br>
当你准备更新动画时你应该调用此方法。这将使浏览器在下一次重绘之前调用你传入给该方法的动画函数(即你的回调函数)。回调函数执行次数通常是每秒`60`次，但在大多数遵循`W3C`建议的浏览器中，回调函数执行次数通常与浏览器屏幕刷新次数相匹配。

页面最小化了，或者被`Tab`切换关灯了。页面绘制全部停止，资源高效利用。

`css3`不支持滚动条动画,所以滚动条滚动动画可以用`requestAnimationFrame`：
```js
(function smoothscroll(){
  let currentScroll = document.documentElement.scrollTop || document.body.scrollTop;
  if (currentScroll > 0) {
    window.requestAnimationFrame(smoothscroll);
    window.scrollTo (0,currentScroll - (currentScroll/5));
  }
})();
```
#### 下面代码输出结果？为什么？
```js
Function.prototype.a = 'a';
Object.prototype.b = 'b';
function Person(){};
var p = new Person();
console.log('p.a: '+ p.a); // p.a: undefined
console.log('p.b: '+ p.b); // p.b: b
```
#### 下面代码输出结果？为什么？
```js
const person = {
  name: 'aaaa',
  say: function (){
    return function (){
      console.log(this.name);
    };
  }
};
person.say()(); // undefined

const person = {
  name: 'aaaa',
  say: function (){
    return () => {
      console.log(this.name);
    };
  }
};
person.say()(); // aaaa
```
#### 执行顺序
```js
async function async1() {
  console.log(1);
  const result = await async2();
  console.log(3);
}

async function async2() {
  console.log(2);
}

Promise.resolve().then(() => {
  console.log(4);
});

setTimeout(() => {
  console.log(5);
});

async1();
console.log(6);

// 1 -> 2 ->6 -> 4 -> 3 -> 5
// async1函数本身会返回一个Promise，同时await后面紧跟着async2函数返回的Promise
//console.log(3)其实是在async2函数返回的Promise的then语句中执行的，
// then语句本身也会返回一个Promise然后追加到微任务队列中，所以在微任务队列中console.log(3)在console.log(4)后面
```
#### 打印结果
```js
let a = {a: 10};
let b = {b: 10};
let obj = {
  a: 10
};
obj[b] = 20;
console.log(obj[a]);

// 20  解析：obj[b]的时候b被转换成了 [object Object], obj[a] 同样也是，所以是20
```
#### 深拷贝和浅拷贝的实现方式分别有哪些？
浅拷贝：<br>
(1) `Object.assign`的方式 <br>
(2) 通过对象扩展运算符 <br>
(3) 通过数组的`slice`方法 <br>
(4) 通过数组的`concat`方法。<br>
深拷贝：<br>
(1) 通过JSON.stringify来序列化对象 <br>
(2) 手动实现递归的方式。<br>
####  `Object.assign`和对象扩展运算符`...` 浅拷贝还是深拷贝
第一级属性深拷贝，以后级别属性浅拷贝
```js
let a = { b: { c: 1 }, d: 1 };
let b = Object.assign({}, a); // 或者let b = {...a}

b.d = 2;
a // { b: { c: 1 }, d: 1 }

b.b.c = 2
a // { b: { c: 2 }, d: 1 }
```
#### 数组的`slice`方法和`concat`方法是浅拷贝还是深拷贝
第一级属性深拷贝，以后级别属性浅拷贝
```js
let a = [{b: 1}, 2,3,4];
let b = a.slice();

b[0].b = 2
a[0] // {b: 2}

b[1] = 5
a[1]  // 2
```
#### `ES5`继承与`ES6`继承
**ES5：**先创造子类的实例对象`this`，然后再将父类的方法添加到`this`上面（`Parent.apply(this)`）。  <br>
**ES6：**先创造父类的实例对象`this`，（所以必须先调用`super`方法）然后再用子类的构造函数修改`this`。

#### `async`和`defer`
`async` : 并行加载脚本文件，下载完毕立即解释执行代码，不会按照页面上的`script`顺序执行。 <br>
`defer` : 并行下载`js`，会按照页面上的`script`标签的顺序执行，然后在文档解析完成之后执行脚本<br>
`defer` 与 `async` 的相同点是采用并行下载，在下载过程中不会产生阻塞。区别在于执行时机，`async` 是加载完成后自动执行，而 `defer` 需要等待页面完成后执行。

#### 实现一个函数判断数据类型
```js
function getType(obj) {
  if(obj === null) return String(obj);
  return typeof obj === 'object' ? Object.prototype.toString.call(obj).replace('[object ', '').replace(']', '').toLowerCase() : typeof obj
}
```
#### 匹配`elective`后的数字输出
`url`有三种情况<br>
`https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=&local_province_id=33`<br>
`https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=800&local_province_id=33`<br>
`https://www.xx.cn/api?keyword=&level1=&local_batch_id=&elective=800,700&local_province_id=33`<br>
匹配`elective`后的数字输出（写出你认为的最优解法）:<br>
`[] || ['800'] || ['800','700']`
```js
let res = str.match(/elective=(\d+,\d+)/);
let result = res ? res[1].split(',') || []
```
#### 返回值
```js
String('11') == new String('11');
String('11') === new String('11');
```
```js
true // new String() 返回的是对象
// == 的时候，实际运行的是
// String('11') == new String('11').toString();
false
```
#### 如下代码的打印结果
```js
var name = 'Tom';
(function() {
 if (typeof name == 'undefined') {
     var name = 'Jack';
     console.log('Goodbye ' + name);
 } else {
     console.log('Hello ' + name);
 }
})();
// Goodbye Tom  由于var穿透块级作用域，提升至function下面，所以typeof name是undefined
```
```js
var name = 'Tom';
(function() {
 if (typeof name == 'undefined') {
     name = 'Jack';
     console.log('Goodbye ' + name);
 } else {
     console.log('Hello ' + name);
 }
})();
// Hello Tom
```
#### 编程题，请写一个函数，完成以下功能
```js
[1, 2, 3, 5, 7, 8, 10]
'1~3, 5, 7~8, 10'
```
```js
function fn(arr) {
  let result = [];
  let temp = arr[0];
  arr.forEach((value, index) => {
    if(value + 1 !== arr[index + 1]) {
      if(temp !== value) {
        result.push(`${temp}~${value}`)
      } else {
        result.push(`${value}`)
      }
      temp = arr[index + 1]
    }
  })
  return result;
}
```
#### 转换对象
```js
var entry = {
  a: {
  b: {
    c: {
      dd: 'abcdd'
    }
  },
  d: {
    xx: 'adxx'
  },
  e: 'ae'
  }
}

// 要求转换成如下对象
var output = {
'a.b.c.dd': 'abcdd',
'a.d.xx': 'adxx',
'a.e': 'ae'
}
```
```js
function flatObj(obj, parentKey, result = {}) {
  for(let key in obj) {
    if(obj.hasOwnProperty(key)) {
      let keyName = `${parentKey}${key}`;
      if(typeof obj[key] === 'object') {
        flatObj(obj[key], keyName+'.', result)
      } else {
        result[key] = obj[key]
      }
    }
  }
  return result;
}
```
#### 转换对象
```js
var entry = {
  'a.b.c.dd': 'abcdd',
  'a.d.xx': 'adxx',
  'a.e': 'ae'
}

// 要求转换成如下对象
var output = {
  a: {
   b: {
     c: {
       dd: 'abcdd'
     }
   },
   d: {
     xx: 'adxx'
   },
   e: 'ae'
  }
}
```
```js
function map(entry) {
  let obj = {};
  for(let key in entry) {
    let keymap = key.split('.');
    set(obj, keymap, entry[key]);
  }
  return obj;
}
function set(obj, map, val) {
  let tmp;
  if(!obj[map[0]]) obj[map[0]] = {};
  tmp = obj[map[0]];
  for(let i = 1; i < map.length; i++) {
    if(!tmp[map[i]]) tmp[map[i]] = map.length - 1 === i ? val : {};
    tmp = tmp[map[i]]
  }
}
```
#### 输出结果
```js
1 + "1"  // 11 转换成字符串
2 * "2" // 4 转换成数字
[1, 2] + [2, 1] // 1,22,1 转换成字符串
"a" + + "b"  // aNaN +'b' 被转换成数字
```
#### 执行结果
```js
function wait() {
  return new Promise(resolve =>
    setTimeout(resolve, 10 * 1000)
  )
}
async function main() {
  console.time();
  const x = wait();
  const y = wait();
  const z = wait();
  await x;
  await y;
  await z;
  console.timeEnd();
}
main();
```
三个任务发起的时候没有`await`，可以认为是同时发起了三个异步。之后各自`await`任务的结果。结果按最高耗时计算，由于三个耗时一样。所以结果是 `10 * 1000ms`
#### 用 `setTimeout` 实现 `setInterval`，阐述实现的效果与`setInterval`的差异
```js
function mySetInterval() {
  mySetInterval.timer = setTimeout(() => {
    arguments[0]();
    mySetInterval(...arguments);
  }, arguments[1])
}
mySetInterval.clear = function() {
  cleatTimout(mySetInterval.timer)
}
```
#### `js`实现`splice`方法
```js
let arr = [1,2,3,4]
function mySplice() {
  let index = arguments[0];
  let num = arguments[1];
  let len = arguments.length;
  let result = [];
  let content = [];
  if(len == 2) {
    // 删除
    result = arr.slice(0, index).concat(arr.slice(index + num));
    arr = result;
    return arr.slice(index, num);
  } else if(len > 2) {
    for(let i = 2; i < len; i++) {
      content.push(arguments[i]);
    }
    if(num == 0) {
      // 插入
      result = arr.slice(0, index).concat(content, arr.slice(index));
      arr = result;
      // return [];
    } else if(num > 0) {
      // 替换
      result = arr.slice(0, index).concat(content, arr.slice(index + num, arr.length));
      arr = result;
    }
  } else {
    console.error('参数有误')
  }
}
```
#### 递归深入对象
```js
 let dan = {
        type: 'person',
        data: {
            gender: 'male',
            info: {
                id: 22,
                fullname: {
                    first: 'Dan',
                    last: 'Deacon'
                }
            }
        }
    }
    
    deepPick('type', dan); //person
    deepPick('data.info.fullname.first', dan) //Dan
```
```js
function deepPick(fields, object = {}) {
  const [first, ...remaining] = fields.split('.');
  return remaining.length ? deepPick(remaining.join('.'), object[first]) : object[first];
}
```
#### `commonjs`和`ES6`模块循环引用
**common.js**<br>
当模块第二次引用时不会去重复加载，而是执行上次缓存的。<br>
一旦出现某个模块被”循环引用”，就只输出已经执行的部分，还未执行的部分不会输出。<br>
```js
// a.js
exports.done = false;
var b = require('./b.js');
exports.hello = false;
console.log(`在a中b.done的值为：`+b.done);
exports.done = true;
```
```js
// b.js
exports.done = false;
var a = require('./a.js');
console.log(`在b中a.done的值为：`+a.done);
console.log(`在b中a.hello的值为：`+a.hello);
exports.done = true;
```
```js
// main.js
var a = require('./a.js');
var b = require('./b.js');
console.log(a.done,b.done,b.hello);
var a1 = require('./a.js');
var b1 = require('./b.js');
console.log(a1.done,b1.done,b.hello);
```
在`a.js`中，第一行，导出其`done`值为`false`，第二行引用`b.js`，此时进入`b.js`中，第一行导出其`done`值为`false`，第二行引用`a.js`，此时进入`a.js`中，而`a.js`只执行了两行，只输出了一个值即`done`为`false`；所以`b.js`而 第二行的a只能引用到`done`值，`hello`值为`undefined`。所以`b.js`输出<br>
```js
在b中a.done的值为：false
在b中a.hello的值为：undefined
```
`b.js`执行完成后，其`done`值为`true`，然后继续`a.js`第三行，第四行输出：
```js
在a中b.done的值为：true
```
此时`a`中`done`为`true`，`hello`为`false`，而`b.js`只能引用`a`中`done`的值。
所以`console.log(a.done,b.done,b.hello)`;这行输出：`true true undefined`,而再次引用`a`和`b`时，不会再次执行。
```js
var a1 = require('./a.js');
var b1 = require('./b.js');
console.log(a1.done,b1.done,b.hello);
```
只会输出：`true true undefined`<br>
**ES6 import模块循环引用**<br>
`ES6`中对模块的引用不像`commonjs`中那样必须执行`require`进来的模块，而只是保存一个模块的引用而已，因此，即使是循环引用也不会出错。
```js
// even.js
import { odd } from './odd'
export var counter = 0;
export function even(n) {
    counter++;
    return n === 0 || odd(n - 1);
}
export function even2() {
   console.log(1234);
}
```
```js
// odd.js
import { even,  even2 } from './even';
export function odd(n) {
    even2();
    return n != 0 && even(n - 1);
}
```
```js
// main.js
require('babel-core/register');
var even = require('./even');
even.even(6);
console.log(even.counter);
```
运行`main.js`输出： 
```js
1234
1234
1234
```
#### `es6`模块与`commonJS`规范的区别
**commomJS模块：**<br>
1、获得的是缓存值，是对模块的拷贝<br>
2、可以对`commomJS`模块重新赋值<br>
3、可以对对象内部的值进行改变<br>

**es6模块：**<br>
1、获得的是实时的值，是对模块的引用<br>
2、对`es6`模块重新赋值会报错<br>
3、可以对对象内部的值进行改变<br>



AST 相关 https://segmentfault.com/a/1190000016231512?utm_source=tag-newest




