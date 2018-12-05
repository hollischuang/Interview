>精简版答案示例，摘自本题的leetCode官方题解

**141. 环形链表**  
---
[https://leetcode-cn.com/problems/linked-list-cycle/](https://leetcode-cn.com/problems/linked-list-cycle/)  

方法一：哈希表
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

---


**参考资料**  

* 本题leetCode官方题解：  
[https://leetcode-cn.com/articles/linked-list-cycle/](https://leetcode-cn.com/articles/linked-list-cycle/)  

* 本题leetCode英文官方题解：  
[https://leetcode.com/articles/linked-list-cycle/](https://leetcode.com/articles/linked-list-cycle/)  