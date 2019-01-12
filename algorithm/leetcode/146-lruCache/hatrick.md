**146. LRU缓存机制**  
---
[https://leetcode-cn.com/problems/lru-cache/](https://leetcode-cn.com/problems/lru-cache/)  

**思路**  
由于LRU缓存插入和删除操作频繁,使用双向链表维护缓存节点,

“新节点”：凡是被访问（新建/修改命中/访问命中）过的节点,一律在访问完成后移动到双向链表尾部,
保证链表尾部始终为最“新”节点;
“旧节点”：保证链表头部始终为最“旧”节点,LRU策略删除时表现为删除双向链表头部;
从链表头部到尾部,节点访问热度逐渐递增，由于链表不支持随机访问,使用HashMap+双向链表实现LRU缓存;

HashMap中键值对：<key, Node>

双向链表：维护缓存节点Node
```java  
  class LRUCache {
      private int capacity;
      private HashMap<Integer, Node> caches;
      private Node first;
      private Node last;
  
      public LRUCache(int capacity) {
          this.capacity = capacity;
          caches = new HashMap<>(capacity);
      }
  
      public void put(int key, int value) {
          Node node = caches.get(key);
          // 首先得先判断是否存在元素
          if (node == null){
              // 如果不存在，先判断容量
              if (capacity <= caches.size()) {
                  // 不够，先移除最后一个
                  removeLast();
              }
              node = new Node();
              node.key = key;
          }
          node.value = value;
          moveNodeToFirst(node);
          caches.put(key, node);
      }
  
      private void removeLast() {
          if (last != null) {
              caches.remove(last.key);
              // 最后
              last = last.pre;
              if (last != null) {
                  last.next = null;
              } else {
                  first = null;
              }
          }
      }
  
      private void moveNodeToFirst(Node node) {
          if (node == first || node == null) return;
          // 先连接
          if (node.pre != null) {
              node.pre.next = node.next;
          }
          if (node.next != null) {
              node.next.pre = node.pre;
          }
          if (node == last) {
              last = last.pre;
          }
          if (last == null || first == null) {
              last = first = node;
              return;
          }
          node.next = first;
          first.pre = node;
          first = node;
          node.pre = null;
      }
  
      public int get(int key) {
          Node node = caches.get(key);
          if (node == null) return -1;
          moveNodeToFirst(node);
          return node.value;
      }
  
  }
  
  class Node {
      Node next;
      Node pre;
      int key;
      int value;
  }
```  

**参考资料**  
[https://segmentfault.com/a/1190000009084949](https://segmentfault.com/a/1190000009084949)  
