**解数独**
---
https://leetcode.com/problems/sudoku-solver/

这道题其实是有效数独的延伸，要求你为给定的数独图的中所有空位填入合适的数字，并且满足数独图的要求。  

难度其实不大，就是普通的dfs问题。分2个步骤：
1. 递过程：对于每一个空的单元格，尝试从1到9，填入单元格中，并检验是否有效。若有效，则向下递，否则换另一个数填入
2. 归过程：即回溯，当前的单元格恢复为空白

我们还是从（0，0）开始，一直探索到不满足条件，然后回溯，再次向下探索，直到（9，0）为止。因为题目保证了必然有解，所以当探索到（9，0）位置时，说明之前填入的数都是合法的。

```java
    public void solveSudoku(char[][] board) {
        dfs(board, 0, 0);
    }

    /**
     * 递归填充值
     * 每个待填的格子  从1开始到9尝试填充
     * @param board
     * @param row
     * @param col
     * @return
     */
    public boolean dfs(char[][] board, int row, int col) {
        //递归结束条件
        if (row == 9 && col == 0) {
            return true;
        }
        /*
            首先生成下一个格子的合法位置
            在这里处理的目的是为了避免后面的循环部分，每次都要求下一个位置
         */
        int newCol = col + 1, newRow = row;
        //换行
        if (newCol == 9) {
            newCol = 0;
            newRow += 1;
        }
        //如果该单元格不用填充，则直接往前递
        if (board[row][col] != '.') {
            return dfs(board, newRow, newCol);
        }
        //尝试1到9填充
        for (char i = '1'; i <= '9'; i++) {
            if (isValid(board, row, col, i)) {
                //递
                board[row][col] = i;
                //要当前填充有解，直接往上回溯
                if (dfs(board, newRow, newCol)) {
                    return true;
                }
                //归
                board[row][col] = '.';
            }
        }
        return false;
    }

    /**
     * 判断如果放入该值，是否满足合法
     * @param board
     * @param row
     * @param col
     * @param ch
     * @return
     */
    public boolean isValid(char[][] board, int row, int col, char ch) {
        //满足行要求
        for (int i = 0; i < 9; i++) {
            if (i != row && ch == board[i][col]) {
                return false;
            }
        }
        //满足列要求
        for (int i = 0; i < 9; i++) {
            if (i != col && ch == board[row][i]) {
                return false;
            }
        }
        //满足单元格要求
        int startRow = row / 3 * 3;
        int startCol = col / 3 * 3;
        for (int i = startRow; i < startRow + 3; i++) {
            for (int j = startCol; j < startCol + 3; j++) {
                if (i != row && j != col && ch == board[i][j]) {
                    return false;
                }
            }
        }
        return true;
    }
```
#### 复杂度
- 时间复杂度：O(9^k)，k为空白的单元格数
- 空间复杂度：O(1)，虽然深度最大为81，但是是固定值，故可认为是常量级别
