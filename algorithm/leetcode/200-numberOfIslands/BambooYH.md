**岛屿的数量**  
[https://leetcode.com/problems/number-of-islands/](https://leetcode.com/problems/number-of-islands/)

解决方案：  
方法一：**并查集**  
**思路**：  
首先看清题意，1代表陆地，0代表水，连在一起的1属于同一块岛屿，此题要求的是岛屿的数量。这个题是一道很明显的并查集的问题。最后剩余的子集数，就是要求的岛屿数。  
[并查集Wiki](https://zh.wikipedia.org/wiki/%E5%B9%B6%E6%9F%A5%E9%9B%86)  

**算法**   
如果能看出这是一个并查集问题，算法就很容易了。首先建立一个并查集的数据结构。然后遍历矩阵，在遍历的过程中，不断的找属于同一个子集的元素。最终返回子集的个数就可以了。

```
class Solution {
    public int numIslands(char[][] grid) {
        //处理一些边界条件
        if(grid == null) {
            return -1;
        }
        if(grid.length == 0 || grid[0].length == 0)
            return 0;
        int row = grid.length;
        int col = grid[0].length;
        //初始化并查集
        UnionFind uf = new UnionFind(grid);
        for(int i = 0; i < row; i++) {
            for(int j = 0; j < col; j++) {
                //如果是0，就说明是水，不用遍历。
                if(grid[i][j] == '0')
                    continue;
                //将二维矩阵转换成以为数组，p就是二维矩阵中的元素在一维数组中对应的坐标。
                int p = i * col + j;
                int q; 
                //这四个if语句，是遍历当前元素的上下左右。如果属于同一个子集，就将他们合并起来。
                if(i > 0 && grid[i-1][j] == '1') {
                    q = (i-1)*col + j;
                    uf.union(p,q);
                }
                if(i < row-1 && grid[i+1][j] == '1') {
                    q = (i+1)*col + j;
                    uf.union(p,q);
                }
                if(j > 0 && grid[i][j-1] == '1') {
                    q = i * col + j-1;
                    uf.union(p,q);
                }
                if(j < col - 1 && grid[i][j+1] == '1') {
                    q = i*col + j + 1;
                    uf.union(p,q);
                }
            }
        }
        //返回count，这就是最终剩下的子集的数量。
        return uf.count;
    }
    
}
//并查集的数据结构
class UnionFind{
    //用于存储他们的父节点
    public int[] visited = null;
    //用于记录最后子集的个数
    public int count;
    public UnionFind(char[][] grid) {
        int row = grid.length;
        int col = grid[0].length;
        //计算该矩阵中有多少个1
        for(int i = 0; i < row; i++) {
            for(int j = 0; j <col; j++) {
                if(grid[i][j] == '1')
                    count++;
            }
        }
        visited = new int[row*col];
        //刚开始，每个节点的父节点都是本身，需要在遍历的过程中，不断更新父节点。
        for(int i = 0; i < row*col; i++) {
            visited[i] = i;
        }
    }
    //查找p的父节点
    public int find(int p) {
        //查找p的父节点，只不过这个循环可以在查找的过程中，缩减从当前节点到最终父节点的路径的长度,也就是路径压缩。可以自己动手画一下模拟这个过程。
        while(p != visited[p]) {
            visited[p] = visited[visited[p]];
            p = visited[p];
        }
        return p;
    }
    //合并子集，如果p和q他们最终的父节点是相同的，这说明他们属于同一个子集
    public void union(int p , int q) {
        int ans1 = find(p);
        int ans2 = find(q);
        if(ans1 == ans2) 
            return;
        //在这里，更优化的方法是按秩合并，有利于提高复杂度
        visited[ans1] = ans2;
        //因为两个节点属于同一个子集，所以1的数量要减1，这样最终遍历完，count就是还剩下多少个子集。
        count--;
    }
}
```
复杂度分析：  
假设M是数组的行数，N是数组的列数。  
时间复杂度：在十分巨大的时候，近似于O(M*N),它比较复杂，可以看一下并查集的Wiki。   
空间复杂度：O(M*N)，这就是辅助数组的大小。