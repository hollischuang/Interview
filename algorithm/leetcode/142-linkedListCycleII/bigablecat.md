**142. 环形链表 II**  
---
[https://leetcode-cn.com/problems/linked-list-cycle-ii/](https://leetcode-cn.com/problems/linked-list-cycle-ii/)  

* 网友高票Java解法，双指针法  

```java  

    public ListNode detectCycle(ListNode head) {
        //快慢指针都从头结点出发
        ListNode slow = head;
        ListNode fast = head;
        boolean hasCycle = false; //判断是否有环的标识
        while (fast != null && fast.next != null) {
            slow = slow.next;//慢指针每次走一步
            fast = fast.next.next;//快指针每次走两步

            if (slow == fast) { //如果快指针等于慢指针，说明有环
                hasCycle = true;
                break; //跳出循环
            }
        }
        //如果有环，第二次循环找出环的入口
        if (hasCycle) {
            //设从头结点到环入口的长度为len
            //从环入口到快慢指针相遇点的距离为h
            //环的长度为r
            //fast和slow走过的相同路段为len+h
            //fast比slow多走的路段为m(m≥1且m是整数)个r，即m*r
            //设slow走过的路程为s，s=len+h
            //设fast走过的路程为f，f=(len+h)+m*r
            //又知道fast走过的路程是slow的两倍，即f=2s
            //2(len+h) = (len+h) + m*r
            // len+h = m*r
            // len = m*r - h
            // 将公式右边变化以后更好理解
            // len = m*r - h = (m-1)*r + (r-h)
            // 让两个指针分别从头结点和相遇点出发，以相同的速度前进
            // 一个指针走完len距离时，到达环形入口
            // 另一个指针围着环绕了(m-1)圈，并且从h位置出发，走了(r-h)步
            // 第二个指针最后到达的位置为 h+(r-h) = r 正好回到环形起点，即环形的入口
            // 最终两个指针会在环形入口处相遇
            slow = head; //让慢指针从头结点重新出发
            while (slow != fast) { //当两个结点未相遇时循环继续
                //慢指针和快指针各走一步
                slow = slow.next;
                fast = fast.next;
            }
            return slow;//循环结束后返回的结点就是环形入口
        }
        return null;
    }


```  

**复杂度分析**  

时间复杂度：O(n)，
判断是否有环时，循环了n+k次，k是快指针比慢指针多跑的长度
查找环的入口时循环了s次，s是从头结点到环入口的距离

空间复杂度：O(1)，只使用了两个临时变量，空间复杂度为常数O(1)

---

**参考资料**  

* 网友高票Java解法：  
[https://leetcode.com/problems/linked-list-cycle-ii/discuss/44774/Java-O(1)-space-solution-with-detailed-explanation.](https://leetcode.com/problems/linked-list-cycle-ii/discuss/44774/Java-O&#401&#41-space-solution-with-detailed-explanation.)  

* 《数据结构面试 之 单链表是否有环及环入口点 附有最详细明了的图解》:  
[https://www.jianshu.com/p/ef71e04241e4](https://www.jianshu.com/p/ef71e04241e4)  