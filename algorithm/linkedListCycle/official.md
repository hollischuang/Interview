方法一：哈希表
思路

我们可以通过检查一个结点此前是否被访问过来判断链表是否为环形链表。常用的方法是使用哈希表。

算法

我们遍历所有结点并在哈希表中存储每个结点的引用（或内存地址）。  
如果当前结点为空结点 null（即已检测到链表尾部的下一个结点），  
那么我们已经遍历完整个链表，并且该链表不是环形链表。  
如果当前结点的引用已经存在于哈希表中，那么返回 true（即该链表为环形链表）。  

```java  

public boolean hasCycle(ListNode head) {
    Set<ListNode> nodesSeen = new HashSet<>();
    while (head != null) {
        if (nodesSeen.contains(head)) {
            return true;
        } else {
            nodesSeen.add(head);
        }
        head = head.next;
    }
    return false;
}

```  

---

内容来源：  

<a href="https://leetcode-cn.com/articles/linked-list-cycle/" target="_blank" style="cursor:pointer;">https://leetcode-cn.com/articles/linked-list-cycle/</a>  
