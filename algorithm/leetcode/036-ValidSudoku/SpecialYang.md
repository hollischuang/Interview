**有效的数独**
---
https://leetcode.com/problems/valid-sudoku/

此题其实很简单，根本无需采用dfs的方式即可解决，平常的循环做法即可。主要解决如下问题：
1. 保证每一行不出现重复的数字
2. 保证每一列不出现重复的数字
3. 保证每一个子九宫格不出现重复的数字

本题没有要求你解数独，只是单纯的让你判断是否合法，那还不简单，遍历判断呗

### 思路一
循环遍历九宫格，判断每一个已填充数字的单元格是否是合法。
1. 对于行，我们判断该行中除了当前列以外是否出现了重复数字
2. 对于列，我们判断该列中除了当前行以外是否出现了重复数字
3. 对于九宫格，我们判断该九宫格除了当前位置以外是否出现了重复数字

```java
    /**
     * 常规遍历做法
     * @param board
     * @return
     */
    public boolean isValidSudoku1(char[][] board) {
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                //对每一个不是'.'的格子进行判断
                if (board[i][j] != '.'
                        && !isValidSudoku(board, i, j)) {
                    return false;
                }
            }
        }
        return true;
    }

    /**
     * 判断该单元格出现的数字是否是合法的
     * @param board
     * @param row
     * @param col
     * @return
     */
    public boolean isValidSudoku(char[][] board, int row, int col) {
        char ch = board[row][col];
        //行
        for (int i = 0; i < 9; i++) {
            if (i != row && ch == board[i][col]) {
                return false;
            }
        }
        //列
        for (int i = 0; i < 9; i++) {
            if (i != col && ch == board[row][i]) {
                return false;
            }
        }
        //九宫格
        //定位该单元格所处的九宫格的起始行
        int startRow = row / 3 * 3;
        //定位该单元格所处的九宫格的起始列
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
- 时间复杂度：O(n^2)，双重循环嘛。至于判断逻辑则是常量级别
- 空间复杂度：O(1)

### 思路二
思路一其实对于每一个单元格进行判断时的算法常量级别还是有点大的。每次都会从行的开头或者列的开头判断，所以我们可以缓存起来，这样下次判断的时候，即可快速查找是否有响应的值，这时就要用O(1)查询的哈希结构了，**集合**非常适合本情况。  
我们利用3个set，分别代表每行，每列，每个九宫格出现的数字
1. 数字存到**行集合**的形式为：`rows.add(num + " in row " + i)`
2. 数字存到**列集合**的形式为：`cols.add(num + " in col " + j)`
3. 数字存到**九宫格集合**的形式为：`blocks.add(num + "in block " + i / 3 + "-" + j / 3)`

```java
    /**
     * 空间换时间
     * @param board
     * @return
     */
    public boolean isValidSudoku2(char[][] board) {
        Set<String> rows = new HashSet<>();
        Set<String> cols = new HashSet<>();
        Set<String> blocks = new HashSet<>();
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.') {
                    char num = board[i][j];
                    if (!rows.add(num + " in row " + i) ||
                        !cols.add(num + " in col " + j) ||
                        !blocks.add(num + "in block " + i / 3 + "-" + j / 3)) {
                        return false;
                    }
                }
            }
        }
        return true;
    }
```
#### 复杂度
- 时间复杂度：O(n)
- 空间复杂度：O(n)

参考：https://leetcode.com/problems/valid-sudoku/discuss/15472/Short%2BSimple-Java-using-Strings
