#### tow sum
在数组中找到 2 个数之和等于给定值的数字，结果返回 2 个数字在数组中的下标。

```
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1]
```
我们很容易想到用哈希表来解决这个问题
我们遍历到数字 a 时，用 target 减去 a，就会得到 b，若 b 存在于哈希表中，我们就可以直接返回结果了。若 b 不存在，那么我们需要将 a 存入哈希表，好让后续遍历的数字使用。

```js
function towSum(nums, target) {
  let map = new Map();
  for(let i = 0, len = nums.length; i < len; i++) {
    if(map.has(target - nums[i])) {
      return [map.get(target - nums[i]), i];
    }
    map.set(nums[i], i);
  }
  return [];
}
```