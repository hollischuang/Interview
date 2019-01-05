**070. ClimbingStairs**  
---
[https://leetcode-cn.com/problems/climbing-stairs/](https://leetcode-cn.com/problems/climbing-stairs/)  

首先判断该问题是否为dp问题。  
对于台阶问题，由于只能走一步或者两步，所以对于N级台阶，很明显得到，第N级台阶的方法数 = 第N-1级台阶方法数　+ 第N-2级台阶方法数。即第N级的“最优决策”只与N-1的“最优决策”和N-2的“最优决策”有关，即满足最优子结构和无后效性，而本身问题是有界的，因此该问题为dp问题。  
令F(n)表示n级台阶的方法数，则有：
* F(n) = F(n-1) + F(n-2) (n>=3),
* F(1) = 1
* F(2) = 2

方法一：递推式的递归版本

```java  

public class Solution {
    /**
     * 递推公式，使用递归的实现
     * @param n 问题中的台阶数
     * @return 返回总步数
     */
    public int climbStairs(int n) {
        if (n > 2)
            return climbStairs(n - 1) + climbStairs(n - 2);// 递推公式递归

        if (n == 2)
            return 2; // 当n=2
        else if (n == 1)
            return 1;// 当n=1

        return 0;
    }
}

```  

方法二：递推式的递归缓存版

递归中存在重复的计算，例如:当n=5时，
* F(5) = F(4)+F(3)
* F(4) = F(3)+F(2)
此时，F(3)重复计算了一次，因此可以在这个地方添加缓存保存中间值。

```java  

public class Solution {
    /**
     * 递推公式，使用递归的实现，添加缓存(又称记忆化搜索)。
     * @param n 问题中的台阶数
     * @return 返回总步数
     */
    public int climbStairs(int n) {
        int[] cache = new int[n + 1]; // step从1开始，所以为了方便理解，cache从位置1开始缓存数据
        return doClimbStairs(n, cache);
    }

    public int doClimbStairs(int step, int[] cache) {
        if (step == 2)
            return 2; // 当n=2
        else if (step == 1)
            return 1;// 当n=1

        if (cache[step] > 0) // int[] 初始化默认值为全0，若缓存命中则值大于0
            return cache[step]; // 命中直接返回
        else {
            cache[step] = doClimbStairs(step - 1, cache) + doClimbStairs(step - 2, cache);// 未命中，使用递推公式递归
            return cache[step]; //返回结果
        }
    }
}

```  

方法三：递推式非递归版本  
因为F(n) = F(n-1) + F(n-2) (n>=3)。  
所以，该式子的计算过程是可以用数组表示的，数组的位置为n的值是位置为n-1的值与位置n-2的值之和。  
所以算法思路为：顺序遍历数组，取数组当前位置的前两个位置的值相加赋值给当前位置。

```java  

public class Solution {
    // 递推公式，非递归版本
    public int climbStairs(int n) {
        if (n == 1)
            return 1; // 算法思路数组长度至少为2，所以1时直接返回结果
	
        // 构造int[] result;存储爬n个台阶的方法数，为了方便对应，所以长度取n+1
        // 如果不希望每一步的结果都被记录，则只需要int result;存储每个阶段的值，
        // 循环到n结束的时候将最终结果返回即可
        int[] result = new int[n + 1]; 
        int start = 1; // 第1个台阶
        int end = 2; // 第2阶台阶
        result[start] = 1; // 第1个台阶方法数为1
        result[end] = 2;// 第2个台阶方法数为2

        for (int i = 3; i <= n; i++) {
            result[i] = result[i - 1] + result[i - 2]; // 第i个台阶数为前两个台阶的方法数之和
        }

        return result[n];
    }
}

```  

---


**参考资料**  
* 斐波那契数列的数学公式：  
[https://blog.csdn.net/beautyofmath/article/details/48184331](https://blog.csdn.net/beautyofmath/article/details/48184331)
* 官方题解：  
[https://leetcode.com/problems/n-queens/discuss/19805/My-easy-understanding-Java-Solution](https://leetcode.com/problems/n-queens/discuss/19805/My-easy-understanding-Java-Solution)  
