**两个节点的最低公共祖先** 
---
[https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/)  
解决方案：  
方法一：**递归**   
**思路**  
这个题，最容易想到的思路就是，先判断根节点是不是公共祖先，然后在判断根节点的左子节点和右子节点是不是公共祖先。但是这样有一个问题是，如果从上往下依次判断的话，会重复计算，所以最好的方法是从下往上开始判断。  
**算法**  
假设要寻找p和q的公共祖先，在对树的遍历过程中，先判断当前节点root是否为null，或者是否为q和p中的一个，如果是的话，则返回root.如果不是的话，则对其左子树和右子树进行寻找。如果左子树返回的为null，那说明公共祖先一定在右字树。如果右子树返回的为null，说明公共祖先一定在左子树中。如果两个都不为null，说明左子树和右字树都各含有一个节点，则返回当前节点  
**代码**
```
class Solution {
    //后序遍历
    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {
        //如果当前节点为null，或者等于p、q中的一个，则返回当前节点
       if(root == null || root == p || root == q)  return root;
       //在左子树中寻找
        TreeNode left = lowestCommonAncestor(root.left,p,q);
        //在右子树中寻找
        TreeNode right = lowestCommonAncestor(root.right,p,q);       
        //如果左子树返回的为null，那说明公共祖先一定在右子树。如果右子树返回的为null，说明公共祖先一定在左子树中。如果两个都不为null，说明左子树和右字树都各含有一个节点，则返回当前节点 
        return left == null ? right : right == null ? left : root;     
    }
   
}
```
复杂度分析：  
假设树的节点的个数为N  
空间复杂度：O(N)  虽然代码中并没有用额外的空间，但是递归本身需要用到栈，最坏情况下，树的高度就是N  
时间复杂度：O(N)  最坏情况下，需要都访问一遍

方法二：**循环遍历**  
**思路**:  
我们可以先找到从根节点分别到p和q的路径，然后对比两条路径，从下到上，第一个相同的节点就是他们俩的最低公共祖先。实现方式可以有多种。  
**算法**  
从根节点开始找到p和q，在寻找的过程中，将节点和其父节点用hashmap存储，然后根据hashmap，我们可以找到从p和q到根节点的路径。进行对比之后，就可以找到最低公共祖先  
```
class Solution {

    public TreeNode lowestCommonAncestor(TreeNode root, TreeNode p, TreeNode q) {

        //用来辅助遍历
        Deque<TreeNode> stack = new ArrayDeque<>();

        //存节点和其父节点
        Map<TreeNode, TreeNode> parent = new HashMap<>();

        parent.put(root, null);
        stack.push(root);

        // 找p和q
        while (!parent.containsKey(p) || !parent.containsKey(q)) {

            TreeNode node = stack.pop();

            // While traversing the tree, keep saving the parent pointers.
            if (node.left != null) {
                parent.put(node.left, node);
                stack.push(node.left);
            }
            if (node.right != null) {
                parent.put(node.right, node);
                stack.push(node.right);
            }
        }


        Set<TreeNode> ancestors = new HashSet<>();

        // 找到从p到根节点的路径
        while (p != null) {
            ancestors.add(p);
            p = parent.get(p);
        }

        // 找从q到根节点的路径，在寻找的过程中，跟p到根节点的路径进行比对，第一个相同的节点就是最低公共祖先
        while (!ancestors.contains(q))
            q = parent.get(q);
        return q;
    }

}
```
复杂度分析：  
假设树节点数为N  
空间复杂度：O(N)，最坏情况下，树高为N  
时间复杂度：O(N)，最坏情况下，需要都访问一遍  

参考资料
- leetcode官方题解  [leetcode官方题解](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/solution/)  
- leetcode得票最多题解[leetcode discuss](https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/discuss/65225/4-lines-C%2B%2BJavaPythonRuby)