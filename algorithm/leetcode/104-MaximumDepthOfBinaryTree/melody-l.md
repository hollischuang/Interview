**051. 二叉树的最大深度**  
---
[https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/](https://leetcode-cn.com/problems/maximum-depth-of-binary-tree/)  

方法一：递归
采用深度优先搜索递归的方式计算最大长度。

```java  
public class Solution {
    public int maxDepth(TreeNode root) {
        // 判断当前结点是否为空
        if (root == null) {
            // 若为空，则返回长度0
            return 0;
        } else {
            // 若不为空，则深度优先搜索

            // 先递归获取左子树的最大长度，
            int left_height = maxDepth(root.left);
            // 递归获取右子树的最大长度
            int right_height = maxDepth(root.right);

            // 返回当前左右子树中最大的长度+1，加一是为了将自己的长度也算上
            return java.lang.Math.max(left_height, right_height) + 1;
        }
    }
}

// 二叉树的定义
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    TreeNode(int x) {
        val = x;
    }
}

```  

**复杂度分析**  

时间复杂度：
O(n)。每个节点只访问一次，所以为O(n)。  

空间复杂度：
最差的情况，即树的高度即为节点个数，则递归N次，因此是O(n)。最好的情况即完全平衡，树的高度将是log(N)，此时空间复杂度为O(log(N))。

方法二：迭代

依旧是深度优先搜索，使用栈来代替递归。两者区别是，递归计算深度是递归结束回溯阶段通过加一来更新深度；迭代计算深度是每访问到一个节点就更新一次深度。

```java

public class Solution {
    public int maxDepth(TreeNode root) {
        // 声明栈，栈中存入KV，其中key是节点，value是当前节点的深度
        Queue<Pair<TreeNode, Integer>> stack = new LinkedList<>();
        // 若当前节点不为空则入栈
        if (root != null) {
            // 根节点入栈，深度为1
            stack.add(new Pair(root, 1));
        }

        // 需要确定的二叉树的最大深度，
        // 通过不断和每个节点的深度比较来确定二叉树的最大深度
        int depth = 0;
        while (!stack.isEmpty()) {
            // 出栈
            Pair<TreeNode, Integer> current = stack.poll();
            // 获取出栈的节点
            root = current.getKey();
            // 获取出栈节点的深度
            int currentDepth = current.getValue();
            if (root != null) {
                // 比较深度大小，更新二叉树的深度
                depth = Math.max(depth, currentDepth);
                // 左子节点入栈
                stack.add(new Pair(root.left, currentDepth + 1));
                // 右子节点入栈
                stack.add(new Pair(root.right, currentDepth + 1));
            }
        }
        return depth;
    }
}

```

---


**参考资料**  

* 官方中文题解：  
[https://leetcode-cn.com/articles/maximum-depth-of-binary-tree/](https://leetcode-cn.com/articles/maximum-depth-of-binary-tree/)  

* 官方英文题解：  
[https://leetcode.com/articles/maximum-depth-of-binary-tree/](https://leetcode.com/articles/maximum-depth-of-binary-tree/)  
