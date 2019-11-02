### `javascript`类型，有哪些你不知道的细节？
#### 基本类型
```js
String, Number, Boolean, undefined, null, Symbol, Object
```
#### 为什么有的编程规范要求用`void 0`代替`undefined`?
`Undefined` 类型表示未定义，它的类型只有一个值，就是 `undefined`。任何变量在赋值前是 `Undefined` 类型、值为`undefined`，一般我们可以用全局变量`undefined`(就是名为`undefined`的这个变量)来表达这个 值，或者 `void` 运算来把任一一个表达式变成 `undefined` 值。<br/>
但是呢，因为`JavaScript`的代码`undefined`是一个变量，而并非是一个关键字，这是`JavaScript`语言公认的设 计失误之一，所以，我们为了避免无意中被篡改，我建议使用 `void 0` 来获取`undefined`值。<br/>

**null**<br/>
`Undefined`跟 `null` 有一定的表意差别，`null`表示的是:“定义了但是为空”。所以，在实际编程时，我们一 般不会把变量赋值为 `undefined`，这样可以保证所有值为 `undefined` 的变量，都是从未赋值的自然状态。<br/>
`Null` 类型也只有一个值，就是 `null`，它的语义表示空值，与 `undefined` 不同，`null` 是 `JavaScript` 关键字，所 以在任何代码中，你都可以放心用 `null` 关键字来获取 `null` 值。<br/>

**Boolean**<br/>
`Boolean` 类型有两个值， `true` 和 `false`，它用于表示逻辑意义上的真和假，同样有关键字 `true` 和 `false` 来表 示两个值。<br/>

**String**<br/>
`String` 用于表示文本数据。`String` 有最大长度是 `2^53 - 1`，这在一般开发中都是够用的，但是有趣的是，这 个所谓最大长度，并不完全是你理解中的字符数。<br/>
因为`String` 的意义并非“字符串”，而是字符串的 `UTF16` 编码，我们字符串的操作 `charAt、charCodeAt、 length` 等方法针对的都是 `UTF16` 编码。所以，字符串的最大长度，实际上是受字符串的编码长度影响的。<br/>

**Number**<br/>
`Number`类型表示我们通常意义上的“数字”。这个数字大致对应数学中 的有理数，当然，在计算机中，我们有一定的精度限制。<br/>
例外：<br/>
`NaN`;<br/> 
`Infinity`，无穷大;<br/>
`-Infinity`，负无穷大。<br/>
`JavaScript`中有 `+0` 和 `-0`，在加法类运算中它们没有区别，但是除法的场合则需要特 别留意区分，“忘记检测除以`-0`，而得到负无穷大”的情况经常会导致错误，而区分 `+0` 和 `-0` 的方式，正是 检测 `1/x` 是 `Infinity` 还是 `-Infinity`。

同样根据浮点数的定义，非整数的`Number`类型无法用 ==(===也不行) 来比较，一段著名的代码，这也正 是我们第三题的问题<br>
**为什么在JavaScript中，0.1+0.2不能=0.3:**<br/>
```js
console.log( 0.1 + 0.2 == 0.3);  //false
```
这里输出的结果是`false`，说明两边不相等的，这是浮点运算的特点，也是很多同学疑惑的来源，浮点数运 算的精度问题导致等式左右的结果并不是严格相等，而是相差了个微小的值。<br/>
所以实际上，这里错误的不是结论，而是比较的方法，正确的比较方法是使用`JavaScript`提供的最小精度值:
```js
console.log( Math.abs(0.1 + 0.2 - 0.3) <= Number.EPSILON);
```

**Symbol**<br/>
`Symbol` 可以具有字符串类型的描述，但是即使描述相同，`Symbol`也不相等。
```js
 var mySymbol = Symbol("my symbol");
```
一些标准中提到的 `Symbol`，可以在全局的 `Symbol` 函数的属性中找到。例如，我们可以使用 `Symbol.iterator` 来自定义 `for...of` 在对象上的行为:
```js
var o = new Object;
o[Symbol.iterator] = function() {
  var v = 0
  return {
    next: function() {
      return { value: v++, done: v > 10 }
    }
  }
};
for(var v of o) {
  console.log(v); // 0 1 2 3 ... 9
}
```
代码中我们定义了`iterator`之后，用`for(var v of o)`就可以调用这个函数，然后我们可以根据函数的行为，产生一个`for...of`的行为。<br>

**Object**<br/>
`Object` 是 `JavaScript` 中最复杂的类型，也是 `JavaScript` 的核心机制之一<br/>
`JavaScript` 中的“类”仅仅是运行时对象的一个私有属性，而`JavaScript`中是无法自定义类型的<br/>
`JavaScript`中的几个基本类型，都在对象类型中有一个“亲戚”。它们是:<br/>
`Number; String; Boolean; Symbol`。
所以，我们必须认识到 `3` 与 `new Number(3)` 是完全不同的值，它们一个是 `Number` 类型， 一个是对象类型。

`Number、String和Boolean`，三个构造器是两用的，当跟 `new` 搭配时，它们产生对象，当直接调用时，它们表示强制类型转换。
`Symbol` 函数比较特殊，直接用 `new` 调用它会抛出错误，但它仍然是 `Symbol` 对象的构造器。

`JavaScript` 语言设计上试图模糊对象和基本类型之间的关系，我们日常代码可以把对象的方法在基本类型上 使用，比如:
```js
console.log("abc".charAt(0)); //a
```
甚至我们在原型上添加方法，都可以应用于基本类型，比如以下代码，在 `Symbol` 原型上添加了`hello`方法， 在任何 `Symbol` 类型变量都可以调用。
```js
Symbol.prototype.hello = () => console.log("hello");
var a = Symbol("a");
console.log(typeof a); //symbol，a并非对象 a.hello(); //hello，有效
```

**为什么给对象添加的方法能用在基本类型上?**<br>
结论：运算符提供了装箱操作，它会根据基础类型构造一个临时对象，使得 我们能在基础类型上调用对应对象的方法。
#### 类型转换
因为`JS`是弱类型语言，所以类型转换发生非常频繁，大部分我们熟悉的运算都会先进行类型转换。<br>
**StringToNumber**<br>
字符串到数字的类型转换，存在一个语法结构，类型转换支持十进制、二进制、八进制和十六进制，比如:
```js
30; 0b111; 0o13; 0xFF。
```
此外，`JavaScript`支持的字符串语法还包括正负号科学计数法，可以使用大写或者小写的`e`来表示:
```js
1e3; -1e-2。
```
`parseInt` 和 `parseFloat` 并不使用这个转换，所以支持的语法跟这里不尽相同<br>

**NumberToString**<br>
在较小的范围内，数字到字符串的转换是完全符合你直觉的十进制表示<br>

**装箱转换**<br>
每一种基本类型`Number、String、Boolean、Symbol`在对象中都有对应的类，所谓装箱转换，正是把基本类型转换为对应的对象，它是类型转换中一种相当重要的种类。
前文提到，全局的 `Symbol` 函数无法使用 `new` 来调用，但我们仍可以利用装箱机制来得到一个 `Symbol` 对 象，我们可以利用一个函数的`call`方法来强迫产生装箱。
我们定义一个函数，函数里面只有`return this`，然后我们调用函数的`call`方法到一个`Symbol`类型的值上，这 样就会产生一个`symbolObject`。
我们可以用`console.log`看一下这个东西的`type of`，它的值是`object`，我们使用`symbolObject instanceof` 可以看到，它是`Symbol`这个类的实例，我们找它的`constructor`也是等于`Symbol`的，所以我们无论从哪个角度看，它都是`Symbol`装箱过的对象:
```js
var symbolObject = (function(){ return this; }).call(Symbol("a"));
console.log(typeof symbolObject); //object
console.log(symbolObject instanceof Symbol); //true
console.log(symbolObject.constructor == Symbol); //true
```
```js
 var symbolObject = Object(Symbol("a"));
console.log(Object.prototype.toString.call(symbolObject)); //[object Symbol]
```

**拆箱转换**<br>
在`JavaScript`标准中，规定了 `ToPrimitive` 函数，它是对象类型到基本类型的转换(即，拆箱转换)。
对象到 `String` 和 `Number` 的转换都遵循“先拆箱再转换”的规则。通过拆箱转换，把对象变成基本类型， 再从基本类型转换为对应的 `String` 或者 `Number`。<br>
拆箱转换会尝试调用 `valueOf` 和 `toString` 来获得拆箱后的基本类型。如果 `valueOf` 和 `toString` 都不存在，或者没有返回基本类型，则会产生类型错误 `TypeError`。
```js
var o = {
  valueOf : () => {console.log("valueOf"); return {}},
  toString : () => {console.log("toString"); return {}}
}
o*2
// valueOf 
// toString 
// TypeError
```
我们定义了一个对象`o`，`o`有`valueOf`和`toString`两个方法，这两个方法都返回一个对象，然后我们进行`o*2`这 个运算的时候，你会看见先执行了`valueOf`，接下来是`toString`，最后抛出了一个`TypeError`，这就说明了这个拆箱转换失败了。<br>
在 `ES6` 之后，还允许对象通过显式指定 `toPrimitive Symbol` 来覆盖原有的行为
```js
 var o = {
    valueOf : () => {console.log("valueOf"); return {}},
    toString : () => {console.log("toString"); return {}}
}
o[Symbol.toPrimitive] = () => {console.log("toPrimitive"); return "hello"}
console.log(o + "")
// toPrimitive
// hello
```











