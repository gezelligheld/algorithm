实现 strStr() 函数

示例
输入：haystack = "hello", needle = "ll"
输出：2

```js
function strStr(haystack, needle) {
    const len = needle.length;
    if (!len) {
        return 0;
    }

    let left = 0;
    let str = '';
    while (left < haystack.length) {
        str += haystack[left];
        if (str.length === len) {
            if (str !== needle) {
                // 没有匹配到时，去掉第一位，继续叠加进行匹配
                str = str.substring(1);
            }
            else {
                return left - (needle.length - 1);
            }
        }
        left ++;
    }
    return -1;
};
```