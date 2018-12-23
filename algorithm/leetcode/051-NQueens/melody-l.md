**051. NQueens**  
---
[https://leetcode-cn.com/problems/n-queens/](https://leetcode-cn.com/problems/n-queens/)  

方法一：回溯法
```java  

public class Solution {
    public List<List<String>> solveNQueens(int n) {
        // 构造棋盘
        char[][] board = new char[n][n];
        // 初始化棋盘所有的棋子
        for(int i = 0; i < n; i++)
            for(int j = 0; j < n; j++)
                board[i][j] = '.';
        // 初始化结果集
        List<List<String>> resultList = new ArrayList<List<String>>();
        // 从第0列开始搜索所有结果，保存到resultList结果集中
        search(board, 0, resultList);

        return resultList;
    }

    /**
     * 递归获取所有的结果
     * @param board 棋盘
     * @param column 所在列
     * @param resultList 最终结果集
     */
    public void search(char[][] board, int column, List<List<String>> resultList) {
        // 如果最后一列已经遍历完，则将棋盘保存，添加到最终结果集中并返回
        if(column == board.length) {
            resultList.add(construct(board));
            return;
        }

        // 寻找第column列的哪一行适合放置皇后的位置
        for(int i = 0; i < board.length; i++) {
            // 如果在第i行，第column列添加皇后能够满足棋盘的0～column列不冲突
            if(validate(board, i, column)) {
                // 第i行，第column列放置一个皇后
                board[i][column] = 'Q';
                // 继续探索第column+1列适合放置皇后的位置
                search(board, column + 1, resultList);
                // 重置column列的结果，继续探索其他位置的可能性
                board[i][column] = '.';
            }
        }
    }

    /**
     * 验证将皇后放在第y列，第x行，是否可行
     * 由于棋盘的0～y-1列的皇后位置都是有效的，
     * 所以验证方式为：让0～y-1列的皇后与当前的皇后比，若都不存在冲突，则有效
     * 冲突检测，正负对角线（测试斜率是否为正负一），是否同一行（不可能同列）
     * @param board 棋盘
     * @param x 所在行
     * @param y 所在列
     * @return 位置是否有效
     */
    public boolean validate(char[][] board, int x, int y) {
        // 从第i行开始遍历
        for(int i = 0; i < board.length; i++) {
            // 从第j列开始遍历
            for(int j = 0; j < y; j++) {
                // 如果此位置为皇后，且满足两点斜率为正负1或者同行，则存在冲突
                if(board[i][j] == 'Q' && (x - i == y - j || x - i == j - y || x == i))
                    return false;
            }
        }

        return true;
    }

    // 产生了一个结果集序列
    public List<String> construct(char[][] board) {
        List<String> result = new LinkedList<String>();
        for(int i = 0; i < board.length; i++) {
            String s = new String(board[i]);
            result.add(s);
        }
        return result;
    }
}

```  

---


**参考资料**  

* 网友推荐题解：  
[https://leetcode.com/problems/n-queens/discuss/19805/My-easy-understanding-Java-Solution](https://leetcode.com/problems/n-queens/discuss/19805/My-easy-understanding-Java-Solution)  
