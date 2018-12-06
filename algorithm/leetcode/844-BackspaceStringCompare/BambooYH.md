**844 比较含退格的字符串**
---
[https://leetcode.com/problems/backspace-string-compare/](https://leetcode.com/problems/backspace-string-compare/)

解决方案  
方法一：栈  
**思路**  
根据题意的描述，我们很容易的会想到最简单直接的方法，就是模拟退格操作，然后得到处理后的字符串，最后比较两个字符串是否相等。用栈来模拟退格操作是合适的，当然也可以用StringBuilder等方法，只要能模拟这个操作就可以，这里我们选用栈来实现  
**算法**  
从左到右遍历字符串，遇到字母，直接压入栈；如果遇到“#”，若当前栈非空，则弹出栈顶元素。  
**代码**  
```
class Solution {
    public boolean backspaceCompare(String S, String T) {
        Stack<Character> stack1 = new Stack();
        //比较两个字符串是否相等
        return help(S,stack1).equals(help(T,stack1));
    }
    public static String help(String S,Stack<Character> stack) {
        //清空一下栈，防止受到上一次运行结果的影响
        stack.clear();
        for(int i = 0; i < S.length(); i++) {
            //如果遇到“#”，当前栈非空，则弹出栈顶元素，既模拟退格操作
            if(S.charAt(i) == '#') {
                if(!stack.empty()) {
                    stack.pop();
                }
                //如果遇到字母，则压栈
            }else {
                stack.push(S.charAt(i));
            }
        }
        return String.valueOf(stack);
    }
}
```  
**复杂度分析**   
设M是字符串S的长度，N是字符串T的长度   
时间复杂度：O(M+N) 既O(Max(M,N))，需要将两个字符串都遍历一遍   
空间复杂度：O(M+N) 既O(Max(M,N))，最多可以添加Max(M,N)个元素到栈里
  
方法二：双指针  
**思路**  
前一个方法是从左到右遍历，当我们遍历到一个字符的时候，我们并不能确定它最终是否存在，因为这取决于后面有多少个“#”。但是如果我们倒过来看，从右向左遍历，在遍历到某个字符的时候，我们就可以确定它最终是否存在。  
**算法**  
从右向左，同时遍历两个字符串，当各自确定了一个字符一定会存在的时候，比较两个字符是否相同。如果都相同，则继续遍历，如果不同，则返回false。在处理的过程中，要注意边界条件。  
**代码**
```
class Solution {
    public boolean backspaceCompare(String S, String T) {
        int i = S.length() - 1, j = T.length() - 1;
        int skipS = 0, skipT = 0;//用来记录字符串S和T分别有多少个“#”

        while (i >= 0 || j >= 0) { 
            //找到S中下一个最终会存在的字符
            while (i >= 0) { 
                if (S.charAt(i) == '#') {skipS++; i--;}
                //如果当前字符是字母，并且有剩余的“#”，则跳过当前字符
                else if (skipS > 0) {skipS--; i--;}
                else break;
            }
            //找到T中下一个最终会存在的字符
            while (j >= 0) { 
                if (T.charAt(j) == '#') {skipT++; j--;}
                else if (skipT > 0) {skipT--; j--;}
                else break;
            }
            //如果两个字符不相同，则返回false
            if (i >= 0 && j >= 0 && S.charAt(i) != T.charAt(j))
                return false;
            //如果一个字符串中找到了一个字符，但是另一个字符串已经遍历完了，则返回false
            if ((i >= 0) != (j >= 0))
                return false;
            i--; j--;
        }
        return true;
    }
}
```
  
**复杂度分析**：  
设M是字符串S的长度，N是字符串T的长度    
空间复杂度：O(1)  
时间复杂度：O(M+N)  
**注意：本题的解法都没考虑字符串为null的情况，因为题目限定了字符串长度大于等于1，如果没有限定条件，还要考虑字符串为null的情况**  

**参考资料**  
- 本题leetCode官方题解  
  [https://leetcode.com/problems/backspace-string-compare/solution/](https://leetcode.com/problems/backspace-string-compare/solution/)