请你设计并实现一个满足 LRU (最近最少使用) 缓存 约束的数据结构

- LRUCache(int capacity) 以 正整数 作为容量  capacity 初始化 LRU 缓存
- int get(int key) 如果关键字 key 存在于缓存中，则返回关键字的值，否则返回 -1 。
- void put(int key, int value)  如果关键字  key 已经存在，则变更其数据值  value ；如果不存在，则向缓存中插入该组  key-value 。如果插入操作导致关键字数量超过 capacity ，则应该 逐出 最久未使用的关键字

使用双向链表按序存储键值对，使用哈希表映射某个 key 在链表中的具体位置，获取或修改时将最新的 key 对应的节点放到头部

```js
class DoubleLinked {
  constructor(key, val) {
    this.key = key;
    this.value = val;
    this.prev = null;
    this.next = null;
  }
}

class LRUCache {
  capacity = 0;

  cache = {};

  head = null;

  tail = null;

  constructor(capacity) {
    this.capacity = capacity;
    this.head = new DoubleLinked();
    this.tail = new DoubleLinked();
    this.head.prev = this.tail;
    this.head.next = this.tail;
    this.tail.prev = this.head;
    this.tail.next = this.head;
  }

  get = (key) => {
    if (!(key in this.cache)) {
      return -1;
    }
    const node = this.cache[key];
    // 操作过的存储放到最前面
    this.moveToHead(node);
    return node.value;
  };

  put = (key, value) => {
    if (key in this.cache) {
      const node = this.cache[key];
      node.value = value;
      // 操作过的存储放到最前面
      this.moveToHead(node);
    } else {
      const node = new DoubleLinked(key, value);
      this.addNode(node);
      this.cache[key] = node;
      // 容量超出时删除最后一个节点，即好久都没使用的存储
      if (Object.keys(this.cache).length > this.capacity) {
        const removeNode = this.removeTail();
        delete this.cache[removeNode.key];
      }
    }
  };

  removeNode = (node) => {
    node.prev.next = node.next;
    node.next.prev = node.prev;
  };

  removeTail = () => {
    const node = this.tail.prev;
    this.removeNode(node);
    return node;
  };

  addNode = (target) => {
    target.next = this.head.next;
    target.prev = this.head;
    this.head.next.prev = target;
    this.head.next = target;
  };

  moveToHead = (node) => {
    this.removeNode(node);
    this.addNode(node);
  };
}
```
