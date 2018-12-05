>详尽版答案示例，摘自本题的leetCode官方题解

**141. 环形链表**  
---
[https://leetcode-cn.com/problems/linked-list-cycle/](https://leetcode-cn.com/problems/linked-list-cycle/)  

摘要  

本文适用于初学者。  

它涉及了以下几个概念：链表，哈希表和双指针。

解决方案  

方法一：哈希表  

思路  

我们可以通过检查一个结点此前是否被访问过来判断链表是否为环形链表。

常用的方法是使用哈希表。  

算法

我们遍历所有结点并在哈希表中存储每个结点的引用（或内存地址）。

如果当前结点为空结点 null（即已检测到链表尾部的下一个结点），那么我们已经遍历完整个链表，并且该链表不是环形链表。

如果当前结点的引用已经存在于哈希表中，那么返回 true（即该链表为环形链表）。


```java  

public boolean hasCycle(ListNode head) {
	//新建一个set用于存储从链表中遍历出的结点
    Set<ListNode> nodesSeen = new HashSet<>();
    //如果当前结点不为空，循环继续
    while (head != null) {
        //set中如果已经存在当前结点，说明该链表是环形链表，返回true
        if (nodesSeen.contains(head)) {
            return true;
        } else {
            //否则将当前结点添加到set
            nodesSeen.add(head);
        }
        //将下一个结点赋值给结点缓存head
        head = head.next;
    }
    //链表所有结点遍历结束没有在set里找到重复结点，说明当前链表没有环，返回false
    return false;
}

```  

**复杂度分析**  

时间复杂度：
O(n)， 对于含有 n个元素的链表，我们访问每个元素最多一次。  添加一个结点到哈希表中只需要花费 O(1) 的时间。  

空间复杂度：
O(n)， 空间取决于添加到哈希表中的元素数目，最多可以添加 n 个元素。   

---


**参考资料**  

* 本题leetCode官方题解：  
[https://leetcode-cn.com/articles/linked-list-cycle/](https://leetcode-cn.com/articles/linked-list-cycle/)  

* 本题leetCode英文官方题解：  
[https://leetcode.com/articles/linked-list-cycle/](https://leetcode.com/articles/linked-list-cycle/)  