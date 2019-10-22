#### 基本数据类型
```js
undefined、null、Boolean、String、Number、Symbol
```
#### `JavaScript`原型、原型链?有什么特点？
每个`Function`对象都会在其内部初始化一个属性，就是`prototype`(原型)，当我们访问一个对象的属性时， 如果这个对象内部不存在这个属性，那么他就会去`prototype`里找这个属性，这个`prototype`又会有自己的`prototype`，于是就这样一直找下去，也就是我们平时所说的原型链的概念。<br/>
特点： `JavaScript`对象是通过引用来传递的，我们创建的每个新对象实体中并没有一份属于自己的原型副本。当我们修改原型时，与之相关的对象也会继承这一改变。
#### 如何实现数组的随机排序？
```js
let arr = [1,2,3,4,5,6,7,8,9,10]; 
function randSort1(arr) {
  for(let i = 0, len = arr.length; i < len; i++) {
    let rand = parseInt(Math.random() * len);
    let temp = arr[rand];
    arr[rand] = arr[i];
    arr[i] = temp;
    return arr;
  }
}
```
```js
let arr = [1,2,3,4,5,6,7,8,9,10];
arr.sort(() => {
  return Math.random() - 0.5;
})
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
var test = new NewType("syf","23");
test.colors.push("black");
alert(test.colors);  //"red,blue,green,black"
alert(test.name);   //"syf"
alert(test.age);   //23

// 组合式继承避免了原型链和借用构造函数继承方式的缺陷，融合了他们的优点，成为了js中最常用的继承方式。
```
#### 请尽可能详尽的解释`AJAX`的优缺点
1、最大的一点是页面无刷新，在页面内与服务器通信，给用户的体验非常好。<br/>
2、使用异步方式与服务器通信，不需要打断用户的操作，具有更加迅速的响应能力。<br/>
3、可以把以前一些服务器负担的工作转嫁到客户端，利用客户端闲置的能力来处理，减轻服务器和带宽的负担，节约空间和宽带租用成本，`ajax`的原则是“按需取数据”，可以最大程度的减少冗余请求。<br/>
4、基于标准化的并被广泛支持的技术，不需要下载插件或者小程序。<br/>

`ajax`的缺点:<br/>
`ajax`对浏览器后退机制造成了破坏；安全问题；对搜索引擎的支持比较弱；破坏了程序的异常机制
#### `get`和`post`有什么区别？
其实，`GET`和`POST`本质上两者没有任何区别。他们都是`HTTP`协议中的请求方法。底层实现都是基于`TCP/IP`协议。所谓区别，只是浏览器厂家根据约定，做得限制而已。<br/>
`get`是通过明文发送数据请求，而`post`是通过密文；<br/>
`get`传输的数据量有限，因为`url`的长度有限，`post`则不受限；<br/>
`GET`请求的参数只能是`ASCII`码，所以中文需要`URL`编码，而`POST`请求传参没有这个限制<br/>
`GET`产生一个`TCP`数据包；`POST`产生两个`TCP`数据包。<br/>
对于`GET`方式的请求，浏览器会把`http header`和`data`一并发送出去，服务器响应`200`（返回数据）；而对于`POST`，浏览器先发送`header`，服务器响应`100 continue`，浏览器再发送`data`，服务器响应`200 ok`（返回数据）。





