#### 二叉树的层次遍历

https://leetcode-cn.com/problems/binary-tree-level-order-traversal/

**思路**：

层次遍历需要借助队列这样一个辅助的数据结构.

1.题目给出从左到右访问节点，自然想到的就是BFS，广度优先搜索。每个节点访问且仅访问一次。所以时间复杂度是O(N)，根节点先入队列，然后队列不空，取出头元素，
如果左孩子存在就入队列，否则什么都不做，右孩子同理。直到队列为空，则表示树层次遍历结束。

2.深度优先搜索DFS，也可以做。但是最好还是BFS

代码：（BFS）

```
class Solution:
    def levelOrder(self, root):
        """
        :type root: TreeNode
        :rtype: List[List[int]]
        """
        if not root:
            return []        #若根节点为空，则返回空列表
        result = []     		#模拟一个队列存储节点
        queue = collections.deque()  #双端队列
        queue.append(root)     #首先将根节点入队
        
        while queue:
            level_size = len(queue)	#记录同层节点的个数
            current_level = []            #使用列表来存储同层节点
            
            for _ in range(level_size):
                node = queue.popleft()			#将同层节点依次出队
                current_level.append(node.val)
                if node.left: queue.append(node.left)		#非空左孩子入队
                if node.right: queue.append(node.right)	#非空右孩子入队
            result.append(current_level)
        return result
```

时间复杂度是O(N)
