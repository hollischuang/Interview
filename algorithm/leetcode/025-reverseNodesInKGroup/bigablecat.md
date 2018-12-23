25. k个一组翻转链表  
---

[https://leetcode-cn.com/problems/reverse-nodes-in-k-group/](https://leetcode-cn.com/problems/reverse-nodes-in-k-group/)  

```java  
    /**
     * 递归解法
     * <p>
	 * //定义ListNode如下
	 * public class ListNode {
	 *     int val;
	 *     ListNode next;
	 *     ListNode(int x) { val = x; }
	 * }
     *
     * @param head
     * @param k
     * @return
     */
    public ListNode reverseKGroup(ListNode head, int k) {
        if (head == null || head.next == null) return head;
        ListNode prev = null; //定义一个前驱结点prev，初始值为null
        ListNode next = head.next; //定义当前结点的后继结点next
        ListNode tail = head; //定义尾结点，缓存当前头结点，反转后变成尾结点
        int k0 = k; //定义k0缓存原始k值
        // while循环中的代码每次操作的都是前一个结点的后继结点
        // 当循环至k-1次时，操作的是本组最后一个结点
        // 所以在循环开始前让k=k-1，将循环减少1次，否则会计算下一组的头结点
        k = k - 1;
        while (next != null && k > 0) {
            head.next = prev; //反转当前结点
            //反转完成后为下一轮循环赋值
            prev = head; //当前结点赋值给前驱结点变量prev
            head = next; //后继结点赋值给当前结点变量head
            next = head.next; //获得新的后继结点
            k--;
        }
        //while循环结束后，head是本组结点原顺序的尾结点，翻转后的新头结点
        //对head进行翻转操作，本组结点全部翻转完毕
        head.next = prev;

        //如果k>0说明本组的结点总数少于k
        if (k > 0) {
            //用原始值k0减去剩余的k，得到本组结点的实际个数
            k = k0 - k;
            //重新反转链表
            //因为本组长度小于k，根据题意要保持原有顺序
            //上面的while循环已经做了反转，重新调用reverseKGroup再反转一次回到原有顺序
            head = reverseKGroup(head, k);
        } else {
            k = k0;//k恢复原始值
            //获取下一组的头结点
            next = reverseKGroup(next, k);
            //将本组的尾结点与下一组的头结点连接
            tail.next = next;
        }
        //每次都返回新的头结点
        return head;
    }

```  

**复杂度分析**  

时间复杂度：O(n)，
若链表结点个数为n，k个一组，共n/k组，
方法依次对每组结点进行处理，总共需要(n/k)次，
因为使用了递归调用，所以递归本身的时间复杂度是n/k，
方法中使用了while循环，遍历每组中的k个结点，时间复杂度为k，
最终时间复杂度是(n/k)*k = n

空间复杂度：O(n)，本题中递归的空间复杂度约为n/k，所以空间复杂度是O(n)

---
