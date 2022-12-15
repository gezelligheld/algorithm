判断一个字符串中满足 AB...BA 形式的最长字符

遍历字符串，依次以每个字符为中心向两边扩散，有两种方式

- 序列长度为偶数，以当前字符和下一位字符为中心，向两边扩散
- 序列长度为奇数，以当前字符为中心，向两边扩散

```js
function getLen(index, str, isMiddle) {
  let left = isMiddle ? index - 1 : index;
  let right = index + 1;
  let length = isMiddle ? 1 : 0;
  while (left >= 0 && right < str.length) {
    if (str[left] === str[right]) {
      length += 2;
      left--;
      right++;
    } else {
      break;
    }
  }
  return length;
}

function maxLength(str) {
  let length = 0;
  for (let i = 0; i < str.length; i++) {
    length = Math.max(length, getLen(i, str, false), getLen(i, str, true));
  }
  return length;
}
```
