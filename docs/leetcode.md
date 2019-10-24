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












