**844. 比较含退格的字符串**  
---
[https://leetcode-cn.com/problems/backspace-string-compare/submissions/](https://leetcode-cn.com/problems/backspace-string-compare/submissions/)  

* 官方题解方法2  

```java  
    /**
     * 从后往前反向遍历字符串中的字符，跳过要删除的字符，比较最终结果会出现的有效字符
     *
     * @param S
     * @param T
     * @return
     */
    public boolean backspaceCompare(String S, String T) {
        //i和j是循环次数上界
        int i = S.length() - 1, j = T.length() - 1;
        //skipS和skipT是计数器，统计循环时跳过的字符个数
        int skipS = 0, skipT = 0;
        //遍历两个字符串中的字符，依次对有效字符进行对比
        while (i >= 0 || j >= 0) {
            //遍历字符串S中的字符
            while (i >= 0) { // Find position of next possible char in build(S)
                //如果当前位置字符是'#'退格符号，计数器skipS递增1
                if (S.charAt(i) == '#') {skipS++; i--;}
                //如果当前位置不是退格键且计数器值大于零，让计数器skipS递减1
                else if (skipS > 0) {skipS--; i--;}
                else break;
            }
            //对字符串T做同样的操作
            while (j >= 0) {
                if (T.charAt(j) == '#') {skipT++; j--;}
                else if (skipT > 0) {skipT--; j--;}
                else break;
            }
            // 讲过上述语句的跳过退格符号和被退格删除的字符
            // 分别取出S和T在最终结果对等位置将出现的字符
            // 如果两个字符不相等，说明两个字符串的最终有效结果不等
            if (i >= 0 && j >= 0 && S.charAt(i) != T.charAt(j))
                return false;
            // i >= 0和j >= 0分别判断两个字符串是否遍历结束
            // 如果一个遍历结束两一个没有结束，说明没有结束的字符串包含的有效字符比另一个多
            if ((i >= 0) != (j >= 0))
                return false;
            i--; j--;
        }
        return true;
    }

```  

**复杂度分析**  

时间复杂度: O(M + N)，M和N分别是字符串S和T的长度，遍历两个字符串的所有字符需要M+N次迭代  

空间复杂度: O(1)，没有使用额外的辅助空间，空间复杂度为O(1)  

---


**参考资料**  

* 本题leetCode英文官方题解：  
[https://leetcode.com/articles/backspace-string-compare/](https://leetcode.com/articles/backspace-string-compare/)  
