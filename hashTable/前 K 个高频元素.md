给你一个整数数组 nums 和一个整数 k ，请你返回其中出现频率前 k 高的元素。你可以按 任意顺序 返回答案。

示例
输入: nums = [1,1,1,2,2,3], k = 2
输出: [1,2]

```js
function topKFrequent(nums, k) {
  const map = new Map();
  const res = [];
  for (let i = 0; i < nums.length; i++) {
    map.set(nums[i], (map.get(nums[i]) || 0) + 1);
  }
  for (let item of map) {
    res.push(item);
  }
  res.sort((a, b) => a[1] - b[1]);
  return res.slice(0, k);
}
```
