**98 判断叉查找树是否有效**
---
[https://leetcode.com/problems/validate-binary-search-tree/](https://leetcode.com/problems/validate-binary-search-tree/)

解决方案：  
方法一：**栈、中序遍历**  
**思路**  
首先，我们要知道二叉搜索树的特点。  
- 若左子树不为空，则左子树节点值小于根节点的值
- 若右字树不为空，则右字树节点值大于根节点的值
- 任意节点的左右字树也为二叉搜索树
- 没有键值相等的节点  
- [参考wiki](https://zh.wikipedia.org/zh-cn/%E4%BA%8C%E5%85%83%E6%90%9C%E5%B0%8B%E6%A8%B9)

所以了解了二叉搜索树的特点之后，我们可以知道，中序遍历二叉搜索树，得到的是一个递增的序列。所以此题只需进行中序遍历，判断是否是一个递增序列就可以了。  
**算法**  
采用非递归的方式进行中序遍历，在遍历的过程中，判断是否是一个递增的序列，如果不是，则返回false  
**代码**  
```
class Solution {
    public boolean isValidBST(TreeNode root) {
        if(root == null)
            return true;
        Stack<TreeNode> stack = new Stack();
        TreeNode pre = null;
        while(root != null || !stack.isEmpty()) {  //当左子节点存在的时候，将左子节点加入栈中
            while(root != null) {
                stack.push(root);
                root = root.left;
            }
            //弹出栈顶节点
            root = stack.pop();
            //如果当前节点小于等于前一个节点的值，则返回false
            if(pre != null && root.val <= pre.val) 
                return false;
            //将pre指向当前节点
            pre = root;
            //继续遍历当前节点的右子节点
            root = root.right;
        }
        return true;
    }
}
```
复杂度分析  
空间复杂度：O(h)，h表示当前树的树高，最坏情况下为n，平均为logn  
时间复杂度：O(n), 当前树的节点的总数
