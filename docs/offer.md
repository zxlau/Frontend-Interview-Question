#### 在一个二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```js
function Find(target, array) {
  let rowCount = array.length - 1, i , j;
  for(let i = rowCount, j = 0; i >=0 && j < array[i].length;) {
    if(target === array[i][j]) {
      return true
    } else if(target > array[i][j]) {
      j++;
      continue;
    } else if(target < array[i][j]) {
      i--;
      continue;
    }
  }
  return false;
}
```

#### .请实现一个函数，将一个字符串中的空格替换成`%20`。例如，当字符串为`We Are Happy`.则经过替换之后的字符串为`We%20Are%20Happy`

```js
function replaceSpace(str) {
  return str.replace(/ /g, '%20');
}
```

#### 输入一个链表，从尾到头打印链表每个节点的值。

```js
/*function ListNode(x){
    this.val = x;
    this.next = null;
}*/
function printListFromTailToHead(head)
  if(!head) {
    return 0
  } else {
    let arr = [];
    let curr = head;
    while(curr) {
      arr.push(curr.val);
      curr = curr.next;
    }
    return arr.reverse();
  }
```

#### 输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列`{1,2,4,7,3,5,6,8}`和中序遍历序列`{4,7,2,1,5,3,8,6}`，则重建二叉树并返回。

```js
/*
function treeNode(x) {
  this.val = x;
  this.left = null;
  this.right = null;
}
*/
function reConstructBinaryTree(pre, vin) {
  if(pre.length === 0 || vin.length === 0) return;
  const index = vin.index(pre[0]);
  const left = vin.splice(0, index);
  const right = vin.splice(index+1);
  const node = new treeNode(vin[index]);
  node.left = reConstructBinaryTree(pre.splice(1, index+1), left);
  node.right = reConstructBinaryTree(pre.splice(index+1), right);
  return node;
}

```

#### 用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

```js
var stack1 = [];
var stack2 = [];

function push(node) {
  stack1.push(node);
}
function pop() {
  var temp = stack1.pop();
  while(temp) {
    stack2.push(temp);
    temp = stack1.pop();
  }
  var result = stack2.pop();
  temp = stack2.pop();
  while(temp) {
    stack1.push(temp);
    temp = stack2.pop();
  }
  return result;
}
```

#### 把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。 输入一个非递减排序的数组的一个旋转，输出旋转数组的最小元素。 例如数组`{3,4,5,1,2}`为`{1,2,3,4,5}`的一个旋转，该数组的最小值为1。 NOTE：给出的所有元素都大于0，若数组大小为0，请返回0

```js
function minNumberInRotateArray(rotateArray) {
  let len = rotateArray.length;
  if(len === 0) return 0;
  return Math.min(...rotateArray);
}
```

#### 大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项。n<=39

```js
function Fibonacci(n) {
  if(n === 0) return 0
  let a = 1; 
  let b = 1;
  let temp;
  for(let i = 2; i <= n; i++) {
    temp = b;
    b = a + b;
    a = temp;
  }
  return a;
}
```
#### 一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

```js
function jumpFloor(number) {
  if(number === 1) {
    return 1;
  }
  if(number === 2) {
    return 2;
  }
  let temp = 0;
  let a = 1;
  let b = 2;
  for(let i = 3; i <= number) {
    temp = b;
    b = a + b;
    a = temp;
  }
  return temp;
}
```

#### 一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法

```js
function jumpFloorII(number) {
  return Math.pow(2, number-1);
}
```

#### 我们可以用21的小矩形横着或者竖着去覆盖更大的矩形。请问用n个21的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？

```js
function rectCover(number) {
  if(number === 0) {
    return 0
  }
  if(number === 1) {
    return 1
  }
  if(number === 2) {
    return 2
  }
  let temp = 0;
  let a = 1;
  let b = 2;
  for(let i = 3; i <= number; i++) {
    temp = b;
    b = a + b;
    a = temp;
  }
  return temp;
}
```
#### 给定一个`double`类型的浮点数`base`和`int`类型的整数`exponent`。求`base`的`exponent`次方。
```js
function Power(base, exponent){
  return Math.pow(base, exponent);
}
```
#### 输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。
```js
function reOrderArray(array){
    var result = [];
    var even = [];
    array.forEach(function(item){
        if((item % 2) === 1){
            result.push(item);
        } else {
            even.push(item);
        }
    });
    return result.concat(even);
}
```
