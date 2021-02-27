输入一个链表，输出该链表中倒数第k个节点

例如，给定一个链表: 1->2->3->4->5, 和 k = 2，返回链表 4->5

```js
function getKthFromEnd(head, k) {
    const arr = [];
    let  cur = head;
    while (cur) {
        arr.push(cur);
        cur = cur.next;
    }
    return arr[arr.length - k];
}
```