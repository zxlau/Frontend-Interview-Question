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
























