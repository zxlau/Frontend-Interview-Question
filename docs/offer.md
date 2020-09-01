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


