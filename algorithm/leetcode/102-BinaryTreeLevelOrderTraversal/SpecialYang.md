**二叉树的层序遍历**
---
https://leetcode.com/problems/binary-tree-level-order-traversal/  

其实二叉树层序遍历本身不难，只需一个队列，不断从根节点插入，然后弹出队列，并把其孩子节点再插入即可。你只要保证了从根->下的顺序，从左—>右的顺序，即就保证了层序。空想很难，不妨自己画个简单的二叉树，演练一遍即可。  
**难点在于如何使得按行打印呢**，也就说打印顺序不变，但是要把属于一行的节点放在一起。

### 思路一
还是借助队列，只不过我们每次弹出时，都会记录下当前队列的长度。为什么这样做呢？因为当前长度正是这一层所有的节点数，然后我们再把其他们的所有的孩子节点加入队列，同样下一次弹出时，记录下队列长度，这时又是当前层的节点数。

以上的技巧需要你从根节点开始保证：
1. 根节点入队列
2. 记录当前队列的长度，为1，当前层为1个节点
3.  开始加入根节点左右孩子
4.  记录当前队列的长度，为2，当前层为2个节点
5.  开始加入他们的左右孩子
6.  一直到队列为空

```java
    /**
     * 最不费脑的一个方法，推荐
     * @param root
     * @return
     */
    public List<List<Integer>> levelOrder1(TreeNode root) {
        List<List<Integer>> result = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root == null) {
            return result;
        }
        queue.offer(root);
        while (!queue.isEmpty()) {
            int size = queue.size();
            List<Integer> level = new LinkedList<>();
            while (size-- > 0) {
                TreeNode node = queue.poll();
                level.add(node.val);
                if (node.left != null) {
                    queue.offer(node.left);
                }
                if (node.right != null) {
                    queue.offer(node.right);
                }
            }
            result.add(level);
        }
        return result;
    }
```

#### 思路二
双指针法。  
last指向当前层最后一个，nLast指向下一层最后一个。  
当队列弹出的节点等于last时，说明当前层遍历完毕，这时需更新last为nLast的层。  
nLast的更新则只需在添加孩子时，更新它即可，因为这些操作都是设计到下一层。
```java
    /**
     * 双指针法
     * @param root
     * @return
     */
    public List<List<Integer>> levelOrder2(TreeNode root) {
        List<List<Integer>> result = new LinkedList<>();
        Queue<TreeNode> queue = new LinkedList<>();
        if (root == null) {
            return result;
        }
        queue.offer(root);
        TreeNode last = root, nLast = null;
        List<Integer> level = new LinkedList<>();
        while (!queue.isEmpty()) {
            TreeNode node = queue.poll();
            level.add(node.val);
            if (node.left != null) {
                queue.offer(node.left);
                nLast = node.left;
            }
            if (node.right != null) {
                queue.offer(node.right);
                nLast = node.right;
            }
            if (node == last) {
                last = nLast;
                result.add(level);
                level = new LinkedList<>();
            }
        }
        return result;
    }
```

### 思路三
DFS，我们都知道层次遍历最符合BFS的方式。但就要是搞事情，此解法来自评论区。  
我们DFS时，会带上树高，若存放本层的节点的容器未创建，则创建，否则直接插入。DFS的遍历保证了同一层的左边的节点先于右边的节点。
所以从根节点到叶子节点的过程中，各个节点是插入到不同层的容器里。
```java
    /**
     * DFS 版
     * @param root
     * @return
     */
    public List<List<Integer>> levelOrder3(TreeNode root) {
        List<List<Integer>> result = new ArrayList<List<Integer>>();
        levelHelper(result, root, 0);
        return result;
    }

    public void levelHelper(List<List<Integer>> result, TreeNode node, int height) {
        if (node == null) {
            return;
        }
        if (height >= result.size()) {
            result.add(new LinkedList<>());
        }
        result.get(height).add(node.val);
        levelHelper(result, node.left, height + 1);
        levelHelper(result, node.right, height + 1);
    }
```

参考：https://leetcode.com/problems/binary-tree-level-order-traversal/discuss/33445/Java-Solution-using-DFS