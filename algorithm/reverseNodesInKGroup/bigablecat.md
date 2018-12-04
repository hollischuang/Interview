25. k个一组翻转链表  
---

[https://leetcode-cn.com/problems/reverse-nodes-in-k-group/description/](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/description/)  

```java  
    /**
     * 递归解法
     * <p>
     * 时间复杂度 O(n)
     * 空间复杂度在 O(n/k)与 O(n/k+2)之间
     *
     * @param head
     * @param k
     * @return
     */
    public static ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null) return head;
        ListNode prev = null; //定义一个前驱结点prev，初始值为null
        ListNode next = head.next; //定义当前结点的后继结点next
        ListNode tail = head; //定义尾结点，缓存当前头结点，反转后变成尾结点
        int k0 = k; //定义k0缓存原始k值
        //因为内循环次数要比分组长度少1次，否则会多计算一个结点，所以k先递减一次
        k--;
        while (next != null && k > 0) {
            head.next = prev; //反转当前结点
            //反转完成后为下一轮循环赋值
            prev = head; //当前结点赋值给前驱结点变量prev
            head = next; //后继结点赋值给当前结点变量head
            next = head.next; //获得新的后继结点
            k--;
        }
        //while循环结束后，head是最后一个结点，还没有进行反转操作
        //对最后一个结点进行反转操作
        head.next = prev;
        //如果k>0说明本组的结点总数少于k
        if (k > 0) {
            //用原始值k0减去剩余的k，得到本组结点的实际个数
            k = k0 - k;
            //重新反转链表
            head = reverseKGroup(head, k);
        } else {
            k = k0;//k恢复原始值
            //获取下一组的头结点
            next = reverseKGroup(next, k);
            //将本组的尾结点与下一组的头结点相连接
            tail.next = next;
        }
        return head;
    }

```  

**复杂度分析**  

时间复杂度： O(n)，内循环复杂度n,外循环复杂度大约是n/k，所以最后复杂度是n*n/k=n  

空间复杂度： O(n)，空间复杂度在 O(n/k)与 O(n/k+2)之间

---
