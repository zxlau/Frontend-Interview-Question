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
```
```js
// 改进
function bubbleSort(arr) {
    let len = arr.length-1;
    while(i > 0) {
        let pos = 0;
        for(let i = 0; i < len; i++) {
            if(arr[j] > arr[j+1]) {
                pos = j;
                let temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
        i = pos;
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























