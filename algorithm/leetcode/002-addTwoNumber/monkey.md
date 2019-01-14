**1. AddTwoNumber**
---
[https://leetcode.com/problems/add-two-numbers/](https://leetcode.com/problems/add-two-numbers/)

- 解决思路
  > 题目只是单纯的要求两个数相加求和，那我们直接用迭代的方式来控制对应位置上数字两两相加。需要注意的有两点：一是需要考虑两个个位数相加的进位，超过10之后向前进一，这里用一个变量来保存进位。二是因为两个数字的长度不一定一样，长度短的如果位数不够，则用0来补足。

- code(scala version)
```
  def addTwoNumbers(l1: ListNode, l2: ListNode): ListNode = {

    var ll1 = l1
    var ll2 = l2

    //定义指向头结点的变量
    var result: ListNode = new ListNode()
    //定义一个dummy指针来指向头节点，这样可以任意的移动刚开始指向头节点的变量而不用担心头结点的丢失
    var dummy: ListNode = result

    //保存两个一位数相加后结果的十位数上的值：结果大于等于10则为1，否则为0
    var carry: Int = 0
    var sum, x, y: Int = 0
    while (ll1 != null || ll2 != null) {
      //如果当前链表的当前节点为空，则值为null
      if (ll1 != null) x = ll1.x else x = 0
      if (ll2 != null) y = ll2.x else y = 0
      sum = x + y + carry
      result.next = new ListNode(sum % 10)
      carry = if (sum > 9) 1 else 0
      result = result.next

      //链表当前节点不为空，则向后推移一个节点，若为空，则不变
      if (ll1 != null) ll1 = ll1.next
      if (ll2 != null) ll2 = ll2.next
    }
    //当两个链表中的所以节点都相加完之后，判断最后一个相加的结果是否超过10，超过10的时候需要额外增加一个节点来保存进位的结果
    if (carry == 1) result.next = new ListNode(1)
    //返回dummy指针的next，即结果的头指针
    dummy.next
  }
```
- 时间空间复杂度分析
  > 时间复杂度：O(max(m,n)) m,n分别为两个链表的长度，因为需要相应位置相加，所以需要遍历两个链表
  > 空间复杂度：O(max(m,n)) m,n分别为两个链表的长度，最后结果长度一定跟最大的数长度相同或者比其大一位，我们用对应长度的链表保存。

- 参考资料
[official solution:  https://leetcode.com/problems/add-two-numbers/solution/](https://leetcode.com/problems/add-two-numbers/solution/)
