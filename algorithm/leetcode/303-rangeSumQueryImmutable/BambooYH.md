**区域和检索 - 数组不可变**

[https://leetcode.com/problems/range-sum-query-immutable/](https://leetcode.com/problems/range-sum-query-immutable/)

解决方案:  
方法一：**辅助数组**  
**思路**：  
此题的要求是求区间和，如果按照题意，直接暴力解决，肯定会超时。所以我们要想一个办法，遍历一遍，就能把结果存在一个辅助数组中。然后当我们要求结果的时候，直接取出来就行了。因为是求一个连续区间的和。比如说求[i,j]的和，如果我们知道了[0,j]和[0,i-1]的和，然后相减，就可以得到[i，j]的和  
**算法**：  
我们要求SumRange(i,j),我们可以用`sum[j+1]-sum[i]`这个公式来求,其中sum[i]表示0到i-1的和。所以我们可以遍历一遍，求出sum[i],然后通过上面的式子来取得结果。  
代码：  
```
public class NumArray {
    private int[] sum;

    public NumArray(int[] nums) {
        //初始化数组
        sum = new int[nums.length + 1];
        //遍历数组，求出sum[i]
        for (int i = 0; i < nums.length; i++) {
            sum[i + 1] = sum[i] + nums[i];
        }
    }

    public int sumRange(int i, int j) {
        //两者相减，就是所求的
        return sum[j + 1] - sum[i];
    }
} 

```
复杂度分析  
假设数组长度为N  
空间复杂度：O(N)  
时间复杂度：O(1),每次查询时间复杂度是O(1),计算的时间复杂度O(N)
