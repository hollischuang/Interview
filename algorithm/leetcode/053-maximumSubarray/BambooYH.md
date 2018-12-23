**最大的子数组和**
---
[https://leetcode.com/problems/maximum-subarray/](https://leetcode.com/problems/maximum-subarray/)  

解决方案：  
方法一：**DP**  
**思路**  
找一个数组中，连续子数组的最大和。最暴力的解法就是两重循环，从第一个元素开始，遍历后面的元素，依次计算子数组的和。然后在从第二个元素开始遍历。这样的做法显然时间复杂度比较高，是`O(n^2)`.我们可以用动态规划的思想来解决这个问题。假设有`array[0]~array[n-1]`共n个元素，换个角度来看问题，我们也就是求分别以`array[i]`结尾的子数组的最大和。如果我们求以`array[n-1]`结尾的子数组最大和`Sum(n-1)`，我们可以先求`array[n-2]`结尾的子数组最大和`Sum(n-2)`，如果这个`Sum(n-2)>0`，那么说明这段子数组对后面的数组一定有贡献，因为这个数是个正数。n  
**算法**  
从左向右遍历，依次计算以`array[i]`为结尾的子数组的最大和。在遍历的过程中，用一个变量`max`记录曾经出现过的最大值。遍历完成后返回`max`。  
```
public class Solution {
    public int maxSubArray(int[] A) {
        int n = A.length;
        //创建一个跟原数组等大小的辅助数组dp，dp[i]表示以A[i]结尾的子数组的最大和
        int[] dp = new int[n];
        //初始化dp[0]，因为以A[0]结尾的子数组的最大和就是A[0]
        dp[0] = A[0];
        //记录遍历过程中出现过的最大值
        int max = dp[0];
        
        for(int i = 1; i < n; i++){
            //如果dp[i-1]>0,那么dp[i] = dp[i-1]+A[i],因为dp[i-1]是个正数，所以对dp[i]有贡献
            dp[i] = A[i] + (dp[i - 1] > 0 ? dp[i - 1] : 0);
            //记录当前出现的最大值
            max = Math.max(max, dp[i]);
        }
        
        return max;
    }
}
```
复杂度分析：  
n表示数组的长度  
空间复杂度：O(1)  
时间复杂度：O(n)  

上面定义了一个DP数组是为了方便理解，其实上面的DP数组可以用一个变量`cur`来表示,代码如下：  
```
public class Solution {
    public int maxSubArray(int[] A) {
        int n = A.length;
        int cur = A[0];
        int max = A[0];
        
        for(int i = 1; i < n; i++){
            cur = cur > 0 ? A[i]+cur : A[i];
            max = Math.max(max, cur);
        }
        
        return max;
    }
}
```

复杂度分析：  
n表示数组的长度  
空间复杂度：O(1)  
时间复杂度：O(n)