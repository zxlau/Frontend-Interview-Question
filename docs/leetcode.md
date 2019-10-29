#### 写一个`js`函数，实现对一个数字每`3`位加一个逗号，如输入`100000`， 输出`100,000`（不考虑负数，小数）
```js
let arr = []
function formatMoney(num) {
    let str = parseInt(num).toString();
    s(str)
}
function s(str) {
    if(str.length > 3) {
        arr.push(str.slice(-3));
        s(str.slice(0, -3))
    } else {
        arr.push(str)
    }
}

formatMoney(1234567899981871.2)
console.log(arr.reverse().join(','))
```
#### 给定一个字符串，找出其中无重复字符的最长子字符串长度
```js
function StrLen(str) {
    let result = 1;
    let norepeatStr = '';
    let len = str.length;
    for(let i = 0; i< len; i++) {
        let specStr = str.charAt(i);
        let index = norepeatStr.indexOf(specStr);
        if(index === -1) {
            norepeatStr = norepeatStr+specStr;
            result = result < norepeatStr.length ? norepeatStr.length : result;
        } else {
            norepeatStr = norepeatStr.substr(index+1) + specStr
        }
    }
    return result
}
console.log(StrLen('abrtyewbbcbd'))
```
#### 实现超出整数存储范围的两个大正整数相加
```js
function func(a, b) {
    let n1 = a.toString().length;
    let n2 = b.toString().length;
    let aa = a.toString();
    let bb = b.toString();
    for(let i = 0; i < Math.max(n1, n2) - Math.min(n1, n2); i++) {
        if(n1 > n2) bb = '0' + bb;
        if(n2 > n1) aa = '0' + aa;
    }
    let arr1 = aa.toString().split('').reverse();
    let arr2 = bb.toString().split('').reverse();

    let n = Math.max(n1, n2);
    let result = Array.apply(this, Array(n)).map(() => 0);
    for(let j = 0; j < n; j++) {
        let temp = parseInt(arr1[j] + arr2[j]);
        if(temp > 9) {
            result[j] += temp -10;
            result[j+1] = 1;
        } else {
            result[j] = temp
        }
    }

    console.log(result.reverse().join('').toString())
}

func('1232423232324145132412341234','23452345234')
```
#### 计算出字符串中出现次数最多的字符是什么，出现了多少次？
```js
let s = 'asdfasdfasdfasdfasdfasfdawerwer';
let arr = s.split('');
let result = arr.reduce((a, b) => {
    if(a[b]) {
        a[b]++;
    } else {
        a[b] = 1;
    }
    return a;
},{})
let result2 = Object.entries(result).sort((a, b) => a-b)[0];
```
#### `"123456789876543212345678987654321..."`的第`n`位是什么？
```js
 let k = '1234567898765432';  // 最小循环节
  function getNum(n) {
      console.log(k.charAt(n % k.length-1))
  }
```
#### 请编写一个函数`parseQueryString`，它的用途是把`URL`参数解析为一个对象
```js
function parseQueryString(url) {
  let pos = url.indexOf('?');
  let obj = {};
  if(pos == -1) return obj;
  let urlstring = url.slice(pos+1);
  let urlArr = urlstring.split("&");
  let keyValue = [];
  for(let i = 0; i < urlArr.length; i++) {
      keyValue = urlArr[i].split('=');
      obj[keyValue[0]] = keyValue[1]
  }
  return obj;
}
```
#### 确保字符串的每个单词首字母都大写，其余部分小写
```js
function titleCase(str) {
  let aStr = str.toLowerCase.split(' ');
  for(let i = 0; i < aStr.length; i++) {
      aStr[i] = aStr[i][0].toUpperCase() + aStr[i].slice(1).toLowerCase()
  }
  let oString = aStr.join(' ');
  return oString;
}
```

#### `1000-div`问题
一次性插入`1000`个`div`，如何优化插入的性能。使用`Fragment`
```js
var oFrag=document.createDocumentFragment();

for(var i=0;i<1000;i++){
    var op=document.createElement("div");
    var oText=document.createTextNode(‘i’);
    op.appendChild(oText);
    oFrag.appendChild(op);
}
document.body.appendChild(oFrag);
```
#### `0.1+0.2`精度问题
插件解决`bigNumber`<br/>
新提案 `bigInt`
```js
function add() {
  const args = [...arguments]
  const maxLen = Math.max.apply(
    null,
    args.map(item => {
      const str = String(item).split('.')[1]
      return str ? str.length : 0
    })
  )
  return (
    args.reduce((sum, cur) => sum + cur * 10 ** maxLen, 0) / 10 ** maxLen
  )
}
```
#### 二分法查找代码
```js
function indexOfSorted(arr, n) {
    let st = 0,
        end = arr.length,
        m = Math.floor(st + end) / 2;
    while(end - st > 1) {
        if(n == arr[st]) {
            return st;
        }
        if(n == arr[end]) {
            return end
        }
        if(n == arr[m]) {
            return m
        }
        if(n > arr[m]) {
            st = m;
            m = Math.floor(st + end) / 2;
        }
        if(n < arr[m]) {
            end = m;
            m = Math.floor(st + end) / 2;
        }
    }
    return -1;
}

let array = [1, 4, 7, 8, 12, 34, 67, 88, 99, 100];
console.log(getIndex(array, 12))
```
#### 已知如下数组： `var arr = [ [1, 2, 2], [3, 4, 5, 5], [6, 7, 8, 9, [11, 12, [12, 13, [14] ] ] ], 10]`; 编写一个程序将数组扁平化去并除其中重复部分数据，最终得到一个升序且不重复的数组
```js
function flatten(arr) {
    let result = arr.reduce((res, item) => {
        res.concat(Array.isArray(item) ? flatten(item) : item);
        return res;
    }, []);
    return result;
}
function format(arr) {
    let flattenArray = flatten(arr);
    return Array.from(new Set(flattenArray)).sort((a, b) => a-b)
}
```
#### 数组中增加一个方法`findDuplicate(n)`数组元素出现频率大于`n`，返回这些元素组成的数组
```js
Array.prototype.findDuplicate = function(count) {
  let result = this.reduce((res, item) => {
    let index = res.findIndex(i => i.name == item.name);
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
#### #### 请把两个数组 `['A1', 'A2', 'B1', 'B2', 'C1', 'C2', 'D1', 'D2']` 和 `['A', 'B', 'C', 'D']`，合并为 `['A1', 'A2', 'A', 'B1', 'B2', 'B', 'C1', 'C2', 'C', 'D1', 'D2', 'D']`
```js
let a1 = ['A1', 'A2', 'B1', 'B2', 'C1', 'C2', 'D1', 'D2'];
let a2 = ['A', 'B', 'C', 'D'].map(item => item + '3');
let a3 = [...a1, ...a2].sort().map(item => {
    if(item.includes('3')) {
        return item.split('')[0] 
    }
    return item
})
```
#### 使用迭代的方式实现 `flatten` 函数。
```js
// 迭代
let arr = [1, 2, [3, 4, 5, [6, 7], 8], 9, 10, [11, [12, 13]]];
const flatten = function(arr) {
    while(arr.some(i => Array.isArray(i))) {
        arr = [].concat(...arr);
    }
    return arr
}
// 递归
const flatten = function(arr) {
    return arr.reduce((res, item) => {
        return res.concat(Array.isArray(item) ? flatten(item) : item);
    }, [])
}
```
#### 冒泡排序以及改进
冒泡排序比较任何两个相邻的项，如果第一个比第二个大，则交换它们。
```js
function bubbleSort(arr) {
    for(let i = 0; i < arr.length; i++) {
        for(let j = 0; j < arr.length; j++) {
            if(arr[j] > arr[j+1]) {
                let temp = arr[j];
                arr[j] = arr[j+1];
                a[j+1] = temp
            }
        }
    }
    return arr;
}
// 从内循环减去外循环中已跑过的轮数，就可以避免内循环中所有不必要的比较
function bubbleSort(array) {
    let length = array.length;
    for(let i = 0; i < length; i++) {
        for(let j = 0; j < length-1-i; j++) {
            if(array[j] > array[j+1]) {
                let temp = arr[j];
                arr[j] = arr[j+1];
                a[j+1] = temp
            }
        }
    }
}
```
#### 要求设计 `LazyMan` 类，实现以下功能
```js
LazyMan('Tony');
// Hi I am Tony

LazyMan('Tony').sleep(10).eat('lunch');
// Hi I am Tony
// 等待了10秒...
// I am eating lunch

LazyMan('Tony').eat('lunch').sleep(10).eat('dinner');
// Hi I am Tony
// I am eating lunch
// 等待了10秒...
// I am eating diner

LazyMan('Tony').eat('lunch').eat('dinner').sleepFirst(5).sleep(10).eat('junk food');
// Hi I am Tony
// 等待了5秒...
// I am eating lunch
// I am eating dinner
// 等待了10秒...
// I am eating junk food
```
```js
class LazyManClass {
    constructor(name) {
        console.log(`Hi I am ${name}`);
        setTimeout(() => {
            this.next();
        }, 0);
        this.taskList = [];
    }
    sleep(time) {
        let fn = (() => {
            return () => {
                setTimeout(() => {
                    this.next();
                }, time)
            }
        })(time);
        this.taskList.push(fn);
        return this;
    },
    eat(name) {
        let fn = (() => {
            return () => {
                console.log(`I am eating ${name}`)
            }
        })(name);
        this.taskList.push(fn);
        return this;
    },
    sleepFirst(time) {
        let fn = (() => {
            return () => {
                setTimeout(() => {
                    this.next();
                }, time)
            }
        })(time);
        this.taskList.unshift(fn);
        return this;
    },
    next() {
        let fn = taskList.unshift();
        fn && fn();
    }
}
function LazyMan(name) {
    return new LazyManClass(name);
}
```
#### 随机生成一个长度为 `10` 的整数类型的数组，例如 `[2, 10, 3, 4, 5, 11, 10, 11, 20]`，将其排列成一个新数组，要求新数组形式如下，例如 `[[2, 3, 4, 5], [10, 11], [20]]`
```js
function getRandomInclusive(min, max) {
    return Math.floor(Math.random() * (max - min + 1) + min);
}
let initArr = Array.from({length: 10}).map(item => getRandomInclusive(0, 99));
initArr.sort((a, b) => a-b);
initArr = [...new Set(initArr)];

let obj = {};
initArr.forEach(item => {
    const initNum = Math.floor(item / 10);
    if(!obj[initNum]) obj[initNum] = [];
    obj[initNum].push(item)
});

let result = [];
for(let i in obj) {
    result.push(obj[i]);
}
```
#### 如何把一个字符串的大小写取反（大写变小写小写变大写），例如 ’AbC' 变成 'aBc'
```js
function processString(s) {
    let arr = s.split('');
    let arr2 = arr.map(item => {
        return item === item.toUpperCase() ? item.toLowerCase() : item.toUpperCase()
    });
    return arr2.join('')
}
```
#### 实现一个字符串匹配算法，从长度为` n `的字符串` S `中，查找是否存在字符串`T`，`T` 的长度是 `m`，若存在返回所在位置。
```js
const find = (S, T) => {
    if(S.length < T.length) return -1;
    for(let i = 0; i < S.length - T.length; i++) {
        if(S.slice(i, T.length) === T) {
            return i
        }
    }
    return -1;
}
```
#### 算法题「旋转数组」
给定一个数组，将数组中的元素向右移动 `k`个位置，其中 `k` 是非负数。 示例:<br>
输入: `[1, 2, 3, 4, 5, 6, 7]` 和 `k = 3`<br>
输出: `[5, 6, 7, 1, 2, 3, 4]`<br>
解释:<br>
向右旋转 `1` 步: `[7, 1, 2, 3, 4, 5, 6]`<br>
向右旋转 `2` 步: `[6, 7, 1, 2, 3, 4, 5]`<br>
向右旋转 `3` 步: `[5, 6, 7, 1, 2, 3, 4]`
```js
function fn(arr, k) {
    let t = k % arr.length;
    return arr.slice(-t).concat(arr.slice(0, arr.length - t));
}
```
#### 打印出 `1 - 10000` 之间的所有对称数
```js
[...Array(10000).keys()].filter(item => {
    return item.toString().length > 1 && item === Number(item.toString().split('').reverse().join(''))
})
```
#### 给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。
示例:<br>
输入: `[0,1,0,3,12]`<br>
输出: `[1,3,12,0,0]`<br>
复制代码说明:<br>
必须在原数组上操作，不能拷贝额外的数组。<br>
尽量减少操作次数。<br>
```js
// 算法思维
function moveZeroToLast(arr) {
    let index = 0;
    for (let i = 0, length = arr.length; i < length; i++) {
        if (arr[i] === 0) {
            index++;
        } else if (index !== 0) {
            arr[i - index] = arr[i];
            arr[i] = 0;
        }
    }
    return arr;
}
// js思维
function fn(arr) {
    let zeroLen = arr.filter(i => i == 0)[length];
    return arr.filter(i => i != 0).concat(Array(zeroLen).map(item => 0))
}
```
#### 函数柯里化
```js
function currying(fn, length) {
  length = length || fn.length; 	// 注释 1
  return function (...args) {			// 注释 2
    return args.length >= length	// 注释 3
    	? fn.apply(this, args)			// 注释 4
      : currying(fn.bind(this, ...args), length - args.length) // 注释 5
  }
}
/*
注释 1：第一次调用获取函数 fn 参数的长度，后续调用获取 fn 剩余参数的长度
注释 2：currying 包裹之后返回一个新函数，接收参数为 ...args
注释 3：新函数接收的参数长度是否大于等于 fn 剩余参数需要接收的长度
注释 4：满足要求，执行 fn 函数，传入新函数的参数
注释 5：不满足要求，递归 currying 函数，新的 fn 为 bind 返回的新函数（bind 绑定了 ...args 参数，未执行），新的 length 为 fn 剩余参数的长度
*/
```
#### 请实现一个 `add` 函数，满足以下功能。
```js
add(1); 			// 1
add(1)(2);  	// 3
add(1)(2)(3)；// 6
add(1)(2, 3); // 6
add(1, 2)(3); // 6
add(1, 2, 3); // 6
```
```js
function add() {
    let args = [].slice.call(arguments);
    let fn = function() {
        let fnArgs = [].slice.call(arguments);
        return add.apply(null, args.concat(fnArgs))
    }
    fn.toString = function() {
        return args.reduce((a, b) => a+b)
    }
    return fn;
}
```
#### 给定一个整数数组和一个目标值，找出数组中和为目标值的两个数。
你可以假设每个输入只对应一种答案，且同样的元素不能被重复利用。 示例： 给定 `nums = [2, 7, 11, 15], target = 9`
因为 `nums[0] + nums[1] = 2 + 7 = 9` 所以返回 `[0, 1]`
```js
function fn(nums, target) {
    let res = [];
    loop:
    for(let i = 0; i < nums.length; i++) {
        for(let j = i+1; j < nums.length; j++) {
            if(nums[i] + nums[j] === target) {
                res.push(i,j)
                break loop;
            }
        }
    }
}
```
#### 在输入框中如何判断输入的是一个正确的网址
```js
function isUrl(url) {
    try{
        new Url(url);
        return true;
    } catch(e) {
        return false
    }
}
```
#### 实现一个`convert`方法 把`list`转换成`tree`格式
```js
// 原始 list 如下
let list =[
    {id:1,name:'部门A',parentId:0},
    {id:2,name:'部门B',parentId:0},
    {id:3,name:'部门C',parentId:1},
    {id:4,name:'部门D',parentId:1},
    {id:5,name:'部门E',parentId:2},
    {id:6,name:'部门F',parentId:3},
    {id:7,name:'部门G',parentId:2},
    {id:8,name:'部门H',parentId:4}
];
const result = convert(list, ...);

// 转换后的结果如下
let result = [
    {
      id: 1,
      name: '部门A',
      parentId: 0,
      children: [
        {
          id: 3,
          name: '部门C',
          parentId: 1,
          children: [
            {
              id: 6,
              name: '部门F',
              parentId: 3
            }, {
              id: 16,
              name: '部门L',
              parentId: 3
            }
          ]
        },
        {
          id: 4,
          name: '部门D',
          parentId: 1,
          children: [
            {
              id: 8,
              name: '部门H',
              parentId: 4
            }
          ]
        }
      ]
    },
  ···
];
```
```js
function convert(list) {
    let res = [];
    let map = list.reduce((res, item) => {
        res[item.id] = item;
        return res;
    }, {});
    list.forEach(item => {
        if(item.parentId == 0) {
            res.push(item)
        } else {
            if(item.parentId in map) {
                let parent = map[item.parentId];
                parent.children = parent.children || [];
                parent.children.push(item)
            }
        }
    });
    return res;
}
```
#### 已知数据格式，实现一个函数 `fn` 找出链条中所有的父级 `id`
```js
let data = [
        {
            id: 1,
            name: '湖南',
            children: [
                {
                    id: 11,
                    name: '长沙',
                    children: [
                        {
                            id: 111,
                            name: '天心区'
                        }
                    ]
                },
                {
                    id: 12,
                    name: '湘潭'
                }
            ]
        }
    ]
```
```js
function fn(data, value, array) {
    let arr = Array.from(array);
    for(let i = 0; i < data.length; i++) {
        arr.push(data[i].id);
        if(value === data[i].id) {
            return arr;
        }
        let children = data[i].children;
        if(children && children.length) {
            let result = fn(children, value, arr);
            return result;
        }
        arr.pop();
    }
}

fn(data, 111, []) 
```
#### 算法题
```js
// 用 JavaScript 写一个函数，输入 int 型，返回整数逆序后的字符串。
// 如：输入整型 1234，返回字符串“4321”。要求必须使用递归函数调用，不能用全局变量，输入函数必须只有一个参数传入，必须返回字符串。
function fn(int) {
    let str = '';
    let string = int.toString().split('').reverse().join('');
    let len = string.length;
    let i = 0;
    return (function recursive(s) {
        str += s;
        if(i <= len -1) {
            recursive(string.substr(i++; 1))
        }
        return str;
    })('')
}

```






