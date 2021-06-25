输入一个链表，输出该链表中倒数第k个节点

例如，给定一个链表: 1->2->3->4->5, 和 k = 2，返回链表 4->5

采用快慢指针，快指针先走k步，然后快慢指针一起走，直到快指针到头，此时慢指针就是倒数第k个元素

```js
function getKthFromEnd(head, k) {
    let slow = head;
    let fast = head;

    for (let i = 0; i < k; i ++) {
        fast = fast.next;
    }

    while (fast) {
        fast = fast.next;
        slow = slow.next;
    }
    return slow;
}
```