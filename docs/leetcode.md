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
    let result = 0;
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
// 或者
function StrLen(str) {
  let result = 0;
  let arr = str.split('');
  let norepeatStr = '';
  arr.forEach(item => {
    let index = norepeatStr.indexOf(item);
    if(index === -1) {
      norepeatStr += item;
      result = Math.max(result, norepeatStr.length);
    } else {
      norepeatStr = norepeatStr.substr(index + 1) + item;
    }
  })
  return result;
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
function getIndex(arr, n) {
    let st = 0,
        end = arr.length,
        m = Math.floor((st + end) / 2);
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
            m = Math.floor((st + end) / 2);
        }
        if(n < arr[m]) {
            end = m;
            m = Math.floor((st + end) / 2);
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
#### 快速排序
```js
function quickSort(arr) {
  if(arr.length < 2) {
    return arr;
  }
  let privot = arr[0];
  let privotArr = [];
  let leftArr = [];
  let rightArr = [];
  arr.forEach(item => {
    if(item === privot) {
      privotArr.push(item)
    } else if(item > privot) {
      rightArr.push(item)
    } else {
      leftArr.push(item)
    }
  });
  return quickSort(leftArr).concat(privotArr).concat(quickSort(rightArr));
}
```
#### 选择排序
```js
function selectionSort(arr) {
  let len = arr.length;
  let minIndex;
  let temp;
  for(let i = 0; i < len-1; i++) {
    minIndex = i;
    for(let j = i + 1; j < len; j++) {
      if(arr[j] < arr[minIndex]) {
        // 寻找最小的数
        minIndex = j; // 将最小的数索引保存
      }
    }
    temp = arr[i];
    arr[i] = arr[minIndex];
    arr[minIndex] = temp;
  }
  return arr;
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
        let fn = taskList.shift();
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
```js
// 或者
function currying(fn, ...args) {
  if(args.length >= fn.length) {
    return fn(...args)
  }
  return function(...args1) {
    return currying(fn, ...args, ...args1)
  }
}
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
// 性能更好
function fn(nums, tartget) {
  let map = {};
  let loop = 0;
  let dis;
  while(loop < nums.length) {
    dis = target - nums[loop];
    if(map[dis] != undefined) {
      return [map[dis], loop];
    }
    map[nums[loop]] = loop;
    loop++;
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
const result = convert(list);

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
#### 二叉树
```js
function BinarySearchTree() {
  let Node = function(key) {
    this.key = key;
    this.left = null;
    this.right = null;
  }
  let root = null;
  this.insert = function(key) {
    let newNode = new Node(key);
    if(root == null) {
      root = newNode;
    } else {
      insertNode(root, newNode)
    }
  } 
  // 中序遍历
  this.inOrderTraverseNode = function(node, callback) {
    if(node != null) {
      this.inOrderTraverseNode(node.left, callback);
      callback(node.key);
      this.inOrderTraverseNode(node.right, callback);
    }
  }
  // 先序遍历
  this.inOrderTraverseNode = function(node, callback) {
    if(node != null) {
      callback(node.key);
      this.inOrderTraverseNode(node.left, callback);
      this.inOrderTraverseNode(node.right, callback);
    }
  }
  // 后序遍历
  this.inOrderTraverseNode = function(node, callback) {
    if(node != null) {
      this.inOrderTraverseNode(node.left, callback);
      this.inOrderTraverseNode(node.right, callback);
      callback(node.key);
    }
  }
  //最小值
  this.minNode = function() {
    return minNode(root);
  }
  //最大值
  this.maxNode = function() {
    return maxNode(root);
  }
  // 搜索某个值
  this.search = function(key) {
    return searchNode(root, key)
  }
}
function searchNode(node, key) {
  if(node === null) {
    return false;
  }
  if(key < node.key) {
    return searchNode(node.left, key)
  } else if(key > node.key) {
    return searchNode(node.right, key)
  } else {
    return true
  }
}
function maxNode(node) {
  if(node) {
    while(node && node.right !== null) {
      node = node.right;
    }
    return node.key;
  }
  return null;
}
function minNode(node) {
  if(node) {
    while(node && node.left !== null) {
      node = node.left;
    }
    return node.key;
  }
  return null;
}
function insertNode(node, newNode) {
  if(newNode.key < node.key) {
    if(node.left === null) {
      node.left = newNode;
    } else {
      insertNode(node.left, newNode);
    }
  } else {
    if(node.right === null) {
      node.right = newNode;
    } else {
      insertNode(node.right, newNode);
    }
  }
}
```
#### 二叉树的层次遍历
```js
var levelOrder = function(root) {
    
    const result = [];
    if (!root) return result;

    let quque = [root];
    let temp = [], res = [];   // temp用于存储下一层级的节点，而res用户存储当前层级的值
    let p;
    while(quque.length > 0 || temp.length > 0) {
        if (quque.length === 0) {  // 当队列长度为0，说明当前层级所有节点已经访问过
            result.push(res);    //保存当前层级的值
            quque = temp;    //访问下一层
            temp = [];
            res = [];
        }
        p = quque.shift();
        if (p) {
            res.push(p.val);
            temp.push(p.left);
            temp.push(p.right);
        }
    }

    return result;
};
```

#### 二叉树四种遍历
```js
// 二叉树
const root = {
  val: "A",
  left: {
    val: "B",
    left: {
      val: "D"
    },
    right: {
      val: "E"
    }
  },
  right: {
    val: "C",
    right: {
      val: "F"
    }
  }
};
```
```js
// 前序遍历顺序 根节点->左子树->右子树
// 遍历完的结果为：A -> B -> D -> E -> C -> F
// 递归写法
function preorder(root) {
  if(!root) return;
  console.log(root.val)
  preorder(root.left);
  preorder(root.right);
}

//  中序遍历的顺序为：左子树->根节点->右子树
// 遍历完的结果为：D -> B -> E -> A -> C -> F
// 递归写法
function inorder(root) {
  if(!root) return;
  inorder(root.left);
  console.log(root.val);
  inorder(root.right);
}

// 后序遍历的顺序为：左子树->右子树->根节点
// 遍历完的结果为：D -> E -> B -> F -> C -> A
function postorder(root) {
  if(!root) return;
  postorder(root.left);
  postorder(root.right);
  console.log(root.val)
}

// 层序遍历就是从上到下，从左到右打印二叉树的节点
// 思路：创建一个数组存放结果，一个队列存放二叉树的节点，如果存放二叉树的队列不为空，就重复下面的步骤：
// （1）将队列的第一个节点作为根节点，并放入结果数组中
// （2）如果该根节点的左子树不为空，就将其放入队列中
// （3）如果该根节点的右子树不为空，就将其放入队列中
function levelTraversal(root) {
  if(!root) return [];
  let queue = [root];
  let result = [];
  while(queue.length !== 0) {
    let node = queue.shift();
    result.push(node.val);
    if(node.left) {
      queue.push(node.left);
    }
    if(node.right) {
      queue.push(node.right);
    }
  }
  return result;
}
```

####  判断对称二叉树
```js
// 实现思路:
// 判断根节点相同
// 左子树的右节点和右子树的左节点相同
// 右子树的左节点和左子树的右节点相同
function isSymmetrical(pRoot) {
  return isSymmetricalTree(pRoot, pRoot);
}
function isSymmetricalTree(node1, node2) {
  // 判断是否都为空,如果为对称二叉树，最后会走进这里
  if(!node1 && !node2) return true;
  // 判断是否有一个为空
  if((!node1 && node2) ||(node1 && !node2)) return false;
  // 判断两个节点是否相等
  if(node1.val != node2.val) return false;
  return isSymmetricalTree(node1.left, node2.right) && isSymmetricalTree(node1.right, node2.left)
}
```


#### 如何从`10000`个数中找到最大的`10`个数
```js
// 冒泡排序，快速排序等，好一点的方法就是下面这种
// 初始化长度为10的数组，然后比较剩下的9990个
function findMax(arr) {
  let result = arr.slice(0, 10).sort((a, b) => b-a);
  let restArr = arr.slice(10);
  for(let i = 0; i < restArr.length; i++) {
    if(restArr[i] > result[9]) {
      result.splice(9, 1);
      result.push(restArr[i]);
      result.sort((a, b) => b-a)
    }
  }
  return result
}
```
#### 手动封装一个请求函数，可以设置最大请求次数，请求成功则不再请求，请求失败则继续请求直到超过最大次数
```js
function request(url, body, successCallback, errorCallback, maxCount = 3) {
  return new Promise((resolve, reject) => {
    fetch(url, body)
      .then(resp => successCallback(resp))
      .catch(err => {
        if(maxCount <= 0) return errorCallback('请求超时');
        request(url, body, successCallback, errorCallback, --maxCount)
      })
  })
}
```
#### 二维数组的全排列组合
```js
let array = [[1,2,3,4], [1,2,3,4], [1,2,3,4]];
console.log(getArrByArrs(array))
function getArrByArrs(array) {
  let arr = [''];
  for(let i = 0; i < array.length; i++) {
    arr = getValueByArr(arr, array[i]);
  }
  return arr;
}
function getValueByArr(arr1, arr2) {
  let arr = [];
  for(let i = 0; i < arr1.length; i++) {
    let v1 = arr1[i];
    for(let j = 0; j < arr2.length; j++) {
      let v2 = arr2[j];
      let value = v1 + v2;
      arr.push(value)
    }
  }
  return arr;
}
```
#### 统计 `1 ~ n` 整数中出现 `1` 的次数
```js
function fn(n) {
  let count = 0;
  for(let i = 0; i <= n; i++) {
    count += String(i).split('').filter(i => i === '1').length
  }
  return count
}
```
#### 有一堆扑克牌，将牌堆第一张放到桌子上，再将接下来的牌堆的第一张放到牌底，如此往复；最后桌子上的牌顺序为： (牌底) 1,2,3,4,5,6,7,8,9,10,11,12,13 (牌顶)；问：原来那堆牌的顺序，用函数实现。
```js
function poke(arr) {
  let i = 1
  let out = []
  while (arr.length) {
    if (i % 2) {
      out.push(arr.shift())
    } else {
      arr.push(arr.shift())
    }
    i++
  }
  return out
}   
    

function reverse(arr) {
  let i = 1
  let out = []
  while (arr.length) {
    if (i % 2) {
      out.unshift(arr.pop())
    } else {
      out.unshift(out.pop())
    }
    i++
  }
  return out
}

reverse([1,2,3,4,5,6,7,8,9,10,11,12,13 ])
// [1, 12, 2, 8, 3, 11, 4, 9, 5, 13, 6, 10, 7]
```

#### 给出两个 非空 的链表用来表示两个非负的整数。其中，它们各自的位数是按照 逆序 的方式存储的，并且它们的每个节点只能存储 一位 数字。如果，我们将这两个数相加起来，则会返回一个新的链表来表示它们的和。您可以假设除了数字 0 之外，这两个数都不会以 0 开头。
#### 示例：
```
输入：(2 -> 4 -> 3) + (5 -> 6 -> 4)
输出：7 -> 0 -> 8
原因：342 + 465 = 807
```
```js
  function listNode(val) {
    this.val = val;
    this.next = null;
  }
  function addTwoNumbers(l1, l2) {
    let node = new listNode('head');
    let temp = node; // 哑结点
    let add = 0;
    let sum = 0;
    while(l1 || l2) {
      sum = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + add;
      temp.next = new listNode(sum % 10);
      temp = temp.next;
      add = sum >= 10 ? 1 : 0;
      l1 && (l1 = l1.next);
      l2 && (l2 = l2.next);
    }
    add && (temp.next = new listNode(add));
    return node.next;
  } 
```

#### js 实现36进制
```js
function getNums36() {
  let result = [];
  for(let i = 0; i < 36; i++) {
    if(i <= 9) {
      result.push(i);
    } else {
      result.push(String.fromCharCode(i + 87))
    }
  }
  return result;
}

function scale36(n) {
  let arr = [];
  while(n) {
    let res = n % 36;
    arr.push(res);
    n = parseInt(n / 36);
  }
  return arr;
}
```

#### 合并乱序区间
```js
// 给出一个区间的集合，请合并所有重叠的区间
// let arr = [[1,3],[2,6],[8,10],[15,18]]
// [ [ 1, 6 ], [ 8, 10 ], [ 15, 18 ] ]
function merge(intervals) {
  if(!intervals || !intervals.length) return [];
  intervals.sort((a, b) => a-b); // 按照区间第一位进行排序
  let result = [intervals[0]]; // 排序之后第一个是最小的 [[1, 3]]
  for(let i = 1; i < intervals.length; i++) { // 从第二个开始比较
    let resultLast = result.length -1;
    if(result[resultLast][1] > intervals[i][0]) {
      result[resultLast][1] = Math.max(result[resultLast][1], intervals[i][1]); // 区间重复就进行合并了
    } else {
      result.push(intervals[i])
    }
  }
  return result;
}

```

#### 随意给定一个无序的、不重复的数组data，任意抽取n个数，相加和为sum，也可能无解，请写出该函数。

```js
function getAllCombin(array, n, sum, temp) {
  if(temp.length === n) {
    if(temp.reduce((res, cur) => res += cur, 0) === sum) {
      return temp;
    }
    return null;
  }
  for(let i = 0; i < array.length; i++) {
    let current = array.shift();
    temp.push(current);
    let result = getAllCombin(array, u, sum, temp);
    if(result) {
      return result;
    }
    temp.pop();
    array.push(current)
  }
}
```
#### 二叉树tree ，根节点是root，判断是否存在一条完整路径，其路径上节点的值之和为target，输出布尔值。
```js
function hasPathSum(node, target) {
  if(!node.left && !node.right) {
    return node.val === target;
  }
  return (node.left && hasPathSum(node.left, target - node.val)) || (node.right && hasPathSum(node.right, target - node.val))
}
```

#### 最长公共前缀
```js
function longestCommonPrefix(strs) {
  let result = '';
  if(!strs.length) return result;
  for(let i = 0; i < strs[0].length; i++) {
    let reg = new RegExp(`^${strs[0].slice(0, i + 1)}`);
    let flag = true;
    strs.forEach(item => {
      if(!item.match(reg)) {
        flag = false;
      }
    })
    if(flag) {
      result = strs[0].slice(0, i + 1);
    }
  }
  return result;
}
```
```js
// 无重复字符串
function lengthOfLongestSubstring(s) {
  let result = 0;
  let noRepeatStr = '';
  for(let i = 0; i < s.length; i++) {
    let index = noRepeatStr.indexOf(s[i]);
    if(index === -1) {
      noRepeatStr += s[i];
      result = Math.max(result, noRepeatStr.length);
    } else {
      noRepeatStr = noRepeatStr.substr(index + 1) + s[i];
    }
  }
  return result;
}
// 最长公共前缀
function longestCommonPrefix(strs) {
  if(!strs || strs.length === 0) return '';
  let result = '';
  for(let i = 0; i < strs[0].length; i++) {
    let reg = new RegExp(`^${strs[0].slice(0, i + 1)}`);
    let flag = true;
    strs.forEach(item => {
      if(!item.match(reg)) {
        flag = false;
      }
    });
    if(flag) {
      result = strs[0].slice(0, i + 1);
    }
  }
  return result;
}

// 字符串的排列
// 给定两个字符串 s1 和 s2，写一个函数来判断 s2 是否包含 s1 的排列。
// 换句话说，第一个字符串的排列之一是第二个字符串的子串。
function fn(s1, s2) {
  let arr = Permutation(s1);
  let res = false;
  arr.forEach(item => {
    if(s2.includes(item)) {
      res = true;
    }
  })
  return res;
}

function Permutation(str) {
  // 字符串的全排列
  if(!str || str.length === 0) return [];
  let result = [];
  let arr = str.split('');
  let temp = '';
  ordering(arr);
  result = result.filter((item, index) => {
    return result.indexOf(item) === index;
  });
  return result;
  function ordering(tempArr) {
    if(tempArr.length === 0) {
      result.push(temp);
      return;
    }
    for(let i = 0; i < tempArr.length; i++) {
      temp += tempArr[i];
      insideArr = tempArr.concat();
      insideArr.splice(i, 1);
      ordering(insideArr);
      temp = temp.substring(0, temp.length - 1);
    }
  }
}

// 字符串相乘
function multiply(num1, num2) {
  let l1 = num1.length;
  let l2 = num2.length;
  let post = Array.from({length: l1 + l2}).fill(0);
  for(let i = l1 -1; i >= 0; i--) {
    let s1 = +num1[i];
    for(let j = l2 - 1; j >= 0; j--) {
      let s2 = +num2[j];
      let multi = s1 * s2;
      let sum = post[i + j + 1] + multi;
      post[i + j + 1] = sum % 10;
      post[i + j] += sum / 10 | 0;
    }
  }
  while(post[0] == 0) {
    post.shift();
  }
  return post.length > 0 ? post.join('') : '0';
}

// 翻转字符串里的单词
function reverseWords(s) {
  return s.split(' ').filter(item => !!item).reverse().join(' ');
}

// 简化路径
function simplifyPath(path) {
  if(!path) return;
  let result = [];
  let arr = path.split('/');
  for(let i = 0; i < arr.length; i ++) {
    if(arr[i] === '' || arr[i] === '.') {
      continue;
    } else if(arr[i] === '..') {
      result.pop();
    } else {
      result.push(arr[i]);
    }
  }
  return `/${result.join('/')}`
}

// 复原IP地址
function restoreIpAddresses(s) {
  let res = [];
  const dfs = (subRes, start) => {
    if(subRes.length === 4 && start === s.length) { // 片段满4段，且耗尽所有字符
      res.push(subRes.join('.')); // 拼成字符串，加入解集
    }
    if(subRes.length === 4 && start < s.length) { // 满4段，字符未耗尽，不用往下选了
      return;
    }
    for(let len = 1; len <= 3; len++) { // 枚举出选择，三种切割长度
      if(start + len - 1 > s.length) return; // 加上要切的长度就越界，不能切这个长度
      if(len != 1 && s[start] == '0') return; // 不能切出'0x'、'0xx'
      let str = s.substring(start, start + len); // 当前选择切出的片段
      if(len === 3 && +str > 255) return; // 不能超过255
      subRes.push(str); // 作出选择，将片段加入subRes
      dfs[subRes, start + len]; // 基于当前选择，继续选择，注意更新指针
      subRes.pop(); // 上面一句的递归分支结束，撤销最后的选择，进入下一轮迭代，考察下一个切割长度
    }
  }
  dfs([], 0);
  return res;
}

// 三数之和
// 给你一个包含 n 个整数的数组 nums，判断 nums 中是否存在三个元素 a，b，c ，使得 a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组
function threeSum(nums) {
  let res = [];
  let len = nums.length;
  if(!nums || len === 0) return res;
  nums.sort((a, b) => a-b);
  for(let i = 0; i < nums.length; i++) {
    if(nums[i] > 0) break;
    if(i > 0 && nums[i] === nums[i + 1]) continue;
    let L = i +1;
    let R = len -1;
    while(L < R) {
      const sum = nums[i] + nums[L] + nums[R];
      if(sum === 0) {
        res.push(nums[i], nums[L], nums[R]);
        while(L < R && nums[L] === nums[L+1]) L++;
        while(L < R && nums[R] === nums[R-1]) R--;
        L++;
        R--;
      } else if(sum < 0) {
        R--;
      } else if(sum > 0) {
        L++;
      }
    }
  }
}

// 岛屿最大面积
function maxAreaOfIsland(grid) {
  let x = grid.length;
  let y = grid[0].length;
  let max = 0;
  for(let i = 0; i < x; i++) {
    for(let j = 0; j < y; j++) {
      if(grid[i][j] === 1) {
        max = Math.max(max, cntArea(grid, i, j, x, y));
      }
    }
  }
  return max;
}

function cntArea(grid, i, j, x, y) {
  if(i < 0|| i >= x || j <0 || j >= y || grid[i][j] === 0) return;
  let cnt = 0;
  grid[i][j] =0;
  cnt += cntArea(grid, i + 1, j, x, y);
  cnt += cntArea(grid, i - 1, j, x, y);
  cnt += cntArea(grid, i, j + 1, x, y);
  cnt += cntArea(grid, i, j - 1, x, y);
  return cnt;
}

// 搜索旋转排序数组
function search(nums, target) {
  let start = 0;
  let end = nums.length - 1;
  let mid;
  while(start < end) {
    mid = Math.ceil((start + end) / 2);
    if(nums[start] === target) return start;
    if(nums[end] === target) return end;
    if(nums[mid] === target) return mid;
    if(nums[start] < nums[mid]) {
      // 前面部分有序
      if(nums[start] < target && target < nums[mid]) {
        end = mid - 1;
      } else {
        start = mid + 1;
      }
    } else {
      // 后面部分
      if(nums[mid] < target && target < nums[end]) {
        start = mid + 1;
      } else {
        end = mid - 1;
      }
    }
  }
  return -1;
}

// 最长连续递增序列
function findLengthOfLCIS(nums) {
  let count = 1;
  let res = 1;
  for(let i = 0; i < nums.length; i++) {
    if(nums[i + 1] > nums[i]) {
      count++;
    } else {
      count = 1;
    }
    res = Math.max(res, count);
  }
  return res;
}

// 一位数组的全排列
function permute(arr) {
  let result = [];
  let usedItem = [];
  function main(arr) {
    for(let i = 0; i < arr.length; i++) {
      let ch = arr.splice(i, 1)[0];
      usedItem.push(ch);
      if(arr.length === 0) {
        result.push(usedItem.slice());
      }
      main(arr);
      arr.splice(i, 0, ch);
      usedItem.pop();
    }
    return result;
  }
  return main(arr);
}

// 省份数量
var findCircleNum = function(isConnected) {
  const provinces = isConnected.length;
  const visited = new Set();
  let circles = 0;
  for (let i = 0; i < provinces; i++) {
      if (!visited.has(i)) {
          dfs(isConnected, visited, provinces, i);
          circles++;
      }
  }
  return circles;
};
const dfs = (isConnected, visited, provinces, i) => {
  for (let j = 0; j < provinces; j++) {
      if (isConnected[i][j] == 1 && !visited.has(j)) {
          visited.add(j);
          dfs(isConnected, visited, provinces, j);
      }
  }
};

// 合并区间
function merge(intervals) {
  intervals.sort((a, b) => a-b);
  let result = [intervals[0]];
  for(let i = 0; i < intervals.length; i++) {
    let resultLast = result.length - 1;
    if(result[resultLast][1] > intervals[i][0]) {
      result[resultLast][1] = Math.max(result[resultLast][1], intervals[i][1]);
    } else {
      result.push(intervals[i]);
    }
  }
  return result;
}

// 接雨水
var trap = function(height) {
  let n=height.length;
  if(n===0) return 0;
  let res=0;

  let left_max=[] ,right_max=[];
  //记录左边数组的最大值
  left_max[0]=height[0];
  for(let i=1;i<n;i++){
    left_max[i]=Math.max(left_max[i-1],height[i]);
  }
  //记录右边数组的最大值
  right_max[n-1]=height[n-1];
  for(let i=n-2;i>=0;i--){
    right_max[i]=Math.max(right_max[i+1],height[i]);
  }
  //统计每一列的面积之和
  for(let i=0;i<n;i++){
    res+=Math.min(left_max[i],right_max[i])-height[i];
  }
  return res;
};

// 合并两个有序链表
function mergeTwoLists(l1, l2) {
  let result = new ListNode(0);
  while(l1 && l2) {
    if(l1.val < l2.val) {
      result.next = l1;
      l1 = l1.next;
    } else {
      result.next = l2;
      l2 = l2.next;
    }
    result = result.next;
  }
  if(l1 && !l2) {
    result.next = l1;
  } else if(!l1 && l2) {
    result.next = l2;
  }
  return result;
}

// 反转链表
function reverseList(head) {
  let pre = null;
  let cur = head;
  while(cur != null) {
    let next = cur.next;
    cur.next = pre;
    pre = cur;
    cur = next;
  }
  return pre;
}

// 两数相加 链表
function addTwoNumbers(l1, l2) {
  let result = new ListNode(0);
  let add = 0;
  while(l1 || l2) {
    let sum = (l1.val || 0) + (l2.val || 0) + add;
    result.val = sum % 10;
    add = sum > 9 ? 1 : 0;
    result.next = new ListNode(0);
    l1 && (l1 = l1.next);
    l2 && (l2 = l2.next);
  }
  add && (result.next = new ListNode(add));
  return result;
}

// 链表排序
function sortList(head) {
  if(!head) return;
  let r = [];
  while(head) {
    r.push(head);
    let temp = head.next;
    head.next = null;
    head = temp;
  }
  r.sort((a, b) => a-b).reduce((p, v) => p.next = v);
  return r[0]
}

// 合并 K 个有序链表
function mergeKLists(lists) {
  return lists.reduce((p, n) => {
    while(n) {
      p.push(n);
      n = n.next;
      return p;
    }
  }).sort((a, b) => a.val - b.val).reduce((p, n) => p.next = n);
}
// 二叉树的最近公共祖先
function lowestCommonAncestor(root, p, q) {
  if(!root || root == p || root == q) return root;
  let left = lowestCommonAncestor(root.left, p, q);
  let right = lowestCommonAncestor(root.right, p, q);
  if(!left) return right;
  if(!right) return left;
  return root;
}

// 买卖股票的最佳时机 [7,1,5,3,6,4]
function maxProfit(prices) {
  if(!prices || prices.length == 0) return;
  let min = prices[0];
  let len = prices.length;
  let dp = Array.from({length: len}).fill(0);
  for(let i = 1; i < len; i++) {
    min = Math.min(min, prices[i]);
    dp[i] = Math.max(dp[i - 1], prices[i] - min);
  }
  return dp[dp.length - 1]
}

// 买卖股票的最佳时机2 [7,1,5,3,6,4]
function maxProfit2(prices) {
  let profit = 0;
  for(let i = 0; i < prices.length; i++) {
    if(prices[i] - prices[i - 1] > 0) {
      profit += prices[i] - prices[i - 1];
    }
  }
  return profit;
}

// 最大正方形
var maximalSquare = function(matrix) {
  if (matrix.length === 0) return 0;
  const dp = [];
  const rows = matrix.length;
  const cols = matrix[0].length;
  let max = Number.MIN_VALUE;

  for (let i = 0; i < rows + 1; i++) {
    if (i === 0) {
      dp[i] = Array(cols + 1).fill(0);
    } else {
      dp[i] = [0];
    }
  }

  for (let i = 1; i < rows + 1; i++) {
    for (let j = 1; j < cols + 1; j++) {
      if (matrix[i - 1][j - 1] === "1") {
        dp[i][j] = Math.min(dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]) + 1;
        max = Math.max(max, dp[i][j]);
      } else {
        dp[i][j] = 0;
      }
    }
  }

  return max * max;
};

// 最大子序和
function maxSub(nums) {
  let len = nums.length;
  let max = nums[0];
  let min = 0;
  let sum = 0
  for(let i = 0; i < len; i++) {
    sum += nums[i];
    if(sum - min > max) {
      max = sum - min;
    }
    if(sum < min) {
      min = sum;
    }
  }
  return max;
}

// 三角形最小路径和
var minimumTotal = function(triangle) {
  const r = triangle.length
  for (let i = r - 1; i >= 0; i --) {
      for (let j = triangle[i].length - 1; j >= 0; j --) {
          if (j + 1 !== triangle[i].length) {
              triangle[i - 1][j] = Math.min(triangle[i][j], triangle[i][j + 1]) + triangle[i - 1][j]
          }
      }
  }
  return triangle[0][0]
};

// 平方根
function fn(x) {
  let r = x;
  while(r ** 2 >x) {
    r = Math.floor((r + x / r) / 2);
  }
  return r;
}
```

#### 最长回文子串
```js
function countSubstrings(s) {
  let res = 0;
  for(let i = 0; i < s.length; i++) {
    let str = '';
    let restr = '';
    for(let j = i; j < s.length; j++) {
      str += s[j];
      restr = s[j] + restr;
      if(str === restr) {
        res++;
      }
    }
  }
  return res;
}
```



