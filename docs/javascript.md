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
