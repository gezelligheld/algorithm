请你设计并实现一个满足 LRU (最近最少使用) 缓存 约束的数据结构

- LRUCache(int capacity) 以 正整数 作为容量  capacity 初始化 LRU 缓存
- int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
- void put(int key, int value)  如果关键字  key 已经存在，则变更其数据值  value ；如果不存在，则向缓存中插入该组  key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字

使用双向链表按序存储键值对，使用哈希表映射某个 key 在链表中的具体位置

```js
class DoubleLinkedList {
  constructor(key, value) {
    this.key = key;
    this.value = value;
    this.next = null;
    this.prev = null;
  }
}

class LRUCache {
  head = null;

  tail = null;

  capacity = null;

  cache = {};

  init = (capacity) => {
    this.cache = {};
    // 添加伪头部和伪尾部，这样操作链表时无需判断节点是否存在
    this.head = new DoubleLinkedList();
    this.tail = new DoubleLinkedList();
    this.head.prev = this.tail;
    this.tail.next = this.head;
    this.capacity = capacity;
  };

  // 获取节点并将其移动到头部
  get = (key) => {
    if (!(key in this.cache)) {
      return -1;
    }
    const node = this.cache[key];
    this.moveToHead(node);
    return node;
  };

  // 存放节点并将其移动到头部
  put = (key, value) => {
    if (!(key in this.cache)) {
      const node = new DoubleLinkedList(key, value);
      this.cache[key] = value;
      this.addToHead(node);
      // 超出容量则删掉尾节点
      if (Object.keys(this.cache).length > this.capacity) {
        const node = this.removeTail();
        delete this.cache[node.key];
      }
    } else {
      const node = this.cache[key];
      node.value = value;
      this.moveToHead(node);
    }
  };

  removeNode = (node) => {
    node.prev.next = node.next;
    node.next.prev = node.prev;
  };

  addToHead = (node) => {
    node.prev = this.head;
    node.next = this.head.next;
    this.head.next.prev = node;
    this.head.next = node;
  };

  moveToHead = (node) => {
    this.removeNode(node);
    this.addToHead(node);
  };

  removeTail = () => {
    const node = this.tail.prev;
    this.removeNode(node);
    return node;
  };
}
```
