**二叉树的层序遍历**
---
[https://leetcode.com/problems/binary-tree-level-order-traversal/](https://leetcode.com/problems/binary-tree-level-order-traversal/)

解决方案   
**思路**  
  使用广度优先探索，使用队列。
  若根节点为空，直接返回;
  否则将根节点入队，然后，判断队列是否为空，若不为空，则将队首节点出队，访问，并判断其左右子节点是否为空，若不为空，则压入队列。
  
```
class Solution{
    List<List<Integer>> res=new ArrayList();
    public List<List<Integer>> levelOrder(TreeNode root) {
        if(root==null) return res;                       //边界条件
        Queue<TreeNode> q=new LinkedList();             //创建的队列用来存放结点,泛型注意是TreeNode
        q.add(root);
        while(!q.isEmpty()){                            //队列为空说明已经遍历完所有元素,while语句用于循环每一个层次
            int count=q.size();
            List<Integer> list=new ArrayList();
            while(count>0){                             //遍历当前层次的每一个结点,每一层次的Count代表了当前层次的结点数目
                TreeNode temp=q.peek();
                q.poll();                               //遍历的每一个结点都需要将其弹出
                list.add(temp.val);
                if(temp.left!=null)q.add(temp.left);    //迭代操作,向左探索
                if(temp.right!=null)q.add(temp.right);
                count--;
            }
            res.add(list);
        }
        return res;

    }
}

```  
**复杂度分析**      
时间复杂度：O(NlogN)  
空间复杂度：O(N+M)    

**参考资料**  
   [https://www.cnblogs.com/patatoforsyj/p/9496127.html](https://www.cnblogs.com/patatoforsyj/p/9496127.html)
