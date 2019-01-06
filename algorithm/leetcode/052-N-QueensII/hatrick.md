**52. N皇后 II**  
---

[https://leetcode-cn.com/problems/n-queens-ii/](https://leetcode-cn.com/problems/n-queens-ii/)  

```java  

public class NQueenII {
    private int count = 0;

    public int totalNQueens(int n) {
        int[] x = new int[n];
        queens(x, n, 0);
        return count;
    }

    private void queens(int[] x, int n, int row) {
        for (int i = 0; i < n; i++) {
            //判断是否合法
            if (check(x, n, row, i)) {
                //将皇后放在第row行,第i列
                x[row] = i;
                //如果是最后一行,则输出结果
                if (row == n - 1) {
                    count++;
                    //回溯,寻找下一个结果
                    x[row] = 0;
                    return;
                }
                //寻找下一行
                queens(x, n, row + 1);
                //回溯
                x[row] = 0;
            }
        }
    }

    /**
     * @param x   数组解
     * @param n   棋盘长宽
     * @param row 当前放置行
     * @param col 当前放置列
     * @return
     */
    private boolean check(int[] x, int n, int row, int col) {
        for (int i = 0; i < row; i++) {
            if (x[i] == col || x[i] + i == col + row || x[i] - i == col - row) {
                return false;
            }
        }
        return true;
    }
}

```  
---


**参考资料**  

[https://blog.csdn.net/xygy8860/article/details/46861817](https://blog.csdn.net/xygy8860/article/details/46861817)  
