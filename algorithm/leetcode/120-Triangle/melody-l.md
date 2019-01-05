**120. Triangle**  

---
[https://leetcode-cn.com/problems/triangle/](https://leetcode-cn.com/problems/triangle/)  

* 该问题如果从上到下的角度考虑，即求从根节点到每一个(i, j)的最短路径。  
令F(i,j)表示从根节点开始到(i,j)的最短路径，A(i,j)表示当前位置的值，则  
F(i,j) = min{F(i-1,j), F(i-1,j-1)} + A(i,j)  （其中，若i<0或j<0，则F(i,j)=0）  

* 该问题如果从下往上考虑，即求从底部到以该结点(i,j)为根节点的最短路径。  
令F(i,j)表示从底部节点到以该结点(i,j)为根节点的最短路径，则  
F(i,j) = min{F(i+1,j), F(i+1, j+1)} + A(i,j)  

* 所以，若问题求从顶部到底端每个节点的最短路径，则从上到下考虑；若问题求全局唯一路径，则采用从下往上考虑；因此，该问题采用由下到上的递推式。

---

方法一：从下往上考虑：
这里只是用了一个int[rowSize]的大小进行存储。以leetcode中的例子来简单描述一下思路：  
1. 先存储最后一行[4,1,8,3]
2. 逆序遍历，以6(3,1)为根节点，从4(4,1)与1(4,2)中选择最小值，因此选择1(4,2);
3. 选择1(4,2)后，加上自己的值，存储到[4,1,8,3]的第一个位置中，原因是对于第一个位置，只有6(3,1)会用到，5(3,2)需要比较的是(4,2)和(4,3)
4. 依次类推，倒数第二行遍历完，数列为[7,6,10,  3]
5. 同理，倒数三行遍历完，数列为[9,10,  10,3]
6. 第一行，数列为[11,  10,10,3]
7. 第一个即为最终结果。

```java  

public class Solution {
    //从下层往上层递推
    public int minimumTotal(List<List<Integer>> triangle) {
        int row = triangle.size(); // 获得行数，其中行数与最后一行的个数相等
        int[] result = new int[row];  // 因为行数与最后一行个数一致，所以直接使用row，无需重复计算

        for (int i = 0; i < row; i++) {
            result[i] = triangle.get(row - 1).get(i);// 先将最后一行赋值给结果序列
        }

        for (int i = row - 2; i >= 0; i--) { // 从倒数第二行(row-2)开始，往上层，逐行遍历，遍历至第0层，因此边界为i>=0
            for (int j = 0; j < i + 1; j++) { // 遍历当前行；当前行的数据个数与 i+1 是相等的，因此此处边界为 j<i+1
                // 递推式 F(i,j) = min{F(i+1,j), F(i+1, j+1)} + A(i,j)
                // 当前行元素(i,j)的最短路径由下一层的(i+1,j),(i+1,j+1)决定
                // 而这两个数据，存储在result的[j],[j+1]中
                result[j] = triangle.get(i).get(j) + Math.min(result[j], result[j + 1]);
            }
        }

        return result[0];
    }
}

```  

**参考资料**  
* 网友高票答案1：   
[https://leetcode.com/problems/triangle/discuss/38730/DP-Solution-for-Triangle](https://leetcode.com/problems/triangle/discuss/38730/DP-Solution-for-Triangle)  
* 网友高票答案2：   
[https://leetcode.com/problems/triangle/discuss/38724/7-lines-neat-Java-Solution](https://leetcode.com/problems/triangle/discuss/38724/7-lines-neat-Java-Solution)
