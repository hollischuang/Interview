**547. 朋友圈**  
---
[https://leetcode-cn.com/problems/friend-circles/](https://leetcode-cn.com/problems/friend-circles/)  

```java  

    /**
     * 定义一个并查集的内部类
     */
    class UnionFind {
        //计数器
        private int count = 0;
        //parent结点集合
        //rank深度
        private int[] parent, rank;

        //定义一个并查集构造器
        public UnionFind(int n) {
            //计数器，记录集合中的分组数
            count = n;
            //父结点数组
            parent = new int[n];
            //结点高度，或者说结点的辈分
            //rank值越高，结点越靠近根结点
            rank = new int[n];
            //创建时，让每个结点的父结点都指向自身
            for (int i = 0; i < n; i++) {
                parent[i] = i;
            }
            //初始化完成后，得到这样一个并查集
            //所有结点的高度一致
            //所有结点的父结点等于自身，即每个结点自成一组
            //初始化分组数量为n
        }


        /**
         * 查找指定结点p的根结点
         * @param p
         * @return
         */
        public int find(int p) {
            //如果当前结点的父结点不等于自身
            while (p != parent[p]) {
                //路径压缩，让结点p的父结点指向祖父结点
                parent[p] = parent[parent[p]];
                //让当前结点指向自身的父结点
                p = parent[p];
            }
            return p;
        }

        /**
         * 合并方法
         * 
         * @param p
         * @param q
         */
        public void union(int p, int q) {
            //查找结点p的根结点
            int rootP = find(p);
            //查找结点Q的根结点
            int rootQ = find(q);
            //如果两个根节点相等，说明两个结点已经在同一组
            if (rootP == rootQ) return;
            //比较两个根结点的rank
            if (rank[rootQ] > rank[rootP]) {
                //rootQ的rank值高，说明rootQ离根结点更近
                //让rootP的父结点指向rooQ
                //即rootP加入rootQ的同组
                parent[rootP] = rootQ;
            } else {
                //否则，让rootQ加入rootP同组
                parent[rootQ] = rootP;
                //如果两个结点的rank值相等
                if (rank[rootP] == rank[rootQ]) {
                    // rankP已经成为父结点
                    // 所以让rootP的高度递增
                    rank[rootP]++;
                }
            }
            //实现p和q的分组，计数器递减
            count--;
        }

        //获取count值的方法
        public int count() {
            return count;
        }
    }

    public int findCircleNum(int[][] M) {
        int n = M.length;
        //创建一个矩阵同等长度的并查集uf
        UnionFind uf = new UnionFind(n);
        //双重嵌套循环，遍历矩阵的每一个结点
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                //如果第i个同学和第j个同学互为好友
                //调用uf.union方法将两个同学分到同一个朋友圈
                if (M[i][j] == 1) uf.union(i, j);
            }
        }
        //返回朋友圈的总个数
        return uf.count();
    }

```  

**复杂度分析**  

时间复杂度：O(n^n)，
嵌套循环进行了n*n次循环，
时间复杂度为O(n^n)

空间复杂度：O(n)，
分别定义了大小为n的int数组parent和rank，
空间复杂度为O(2n)，
消去常数项得到O(n)

---

**参考资料**  

* 网友高票Java解法(unionfind)：  
[https://leetcode.com/problems/friend-circles/discuss/101336/Java-solution-Union-Find](https://leetcode.com/problems/friend-circles/discuss/101336/Java-solution-Union-Find)  
