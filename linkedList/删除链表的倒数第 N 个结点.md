给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点

示例
输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]

方法一：先计算出链表长度，再遍历一次找到目标位置的前一个节点，然后删除目标节点

```js
function getLength(head) {
    let cur = head;
    let len = 0;
    while (cur) {
        cur = cur.next;
        len ++;
    }
    return len;
}

function removeNthFromEnd(head, n) {
    const len = getLength(head);
    const node = new ListNode(0, head);

    let cur = node;

    for (let i = 0; i < len - n; i ++) {
        cur = cur.next;
    }

    cur.next = cur.next.next;
    return node.next;
};
```

方法二：先遍历一遍将所有节点推入栈中，利用栈先入后出的特点找到目标节点的前一个节点，然后进行删除

```js
function removeNthFromEnd(head, n) {
    const stack = [];
    const ans = new ListNode(0, head);
    let cur = ans;
    while (cur) {
        stack.push(cur);
        cur = cur.next;
    }

    // 注意这里的限制条件是n，栈先弹出链表后面的节点
    for (let i = 0; i < n; i ++) {
        stack.pop();
    }
    const node = stack.pop();
    node.next = node.next.next;
    return ans.next;
};
```

方法三：快慢指针，快指针先走n步，再一起遍历快慢指针，当快指针遍历结束后，慢指针走到目标节点的前一个节点，然后删除节点

```js
function removeNthFromEnd(head, n) {
    const ans = new ListNode(0, head);
    let slow = ans;
    let fast = head;

    // 快指针先走n步
    for (let i = 0; i < n; i ++) {
        fast = fast.next;
    }

    while (fast) {
        fast = fast.next;
        slow = slow.next;
    }
    slow.next = slow.next.next;
    return ans.next;
};
```