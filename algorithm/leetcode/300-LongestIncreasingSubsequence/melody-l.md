**300. LongestIncreasingSubsequence**  
---
[https://leetcode-cn.com/problems/longest-increasing-subsequence/](https://leetcode-cn.com/problems/longest-increasing-subsequence/)  

方法一：动态规划

思路：从头开始遍历，查找以当前点为数组最后位置的最长子序列。由于当前点前面的所有点的最长子序列已经遍历完成了，所以只是需要找到前面所有子序列的最大值即可。  
即令F(i)表示数组nums的从0到i位的最长子序列长度。则有F(i)=max{F(0)...F(i-1)}+1  
所以代码如下：

```java  
public class Solution {
    //从前往后
    public int lengthOfLIS(int[] nums) {
        if (nums.length == 0) // 测试用例中有集合为空的例子
            return 0;

        int max = 0;// 保存最大值
        int[] result = new int[nums.length];// 保存每一位最长子序列结果的数组，初始化默认值为0
        for (int i = 0; i < nums.length; i++) { // 从左至右顺序遍历每一位
            result[i] = 1; // 对于每一位，其最长子序列至少为一
            for (int j = 0; j < i; j++) {// 从数组开始到当前位置，找寻前面所有的数的最大子序列长度
                // 最大子序列长度寻找标准：
                // 1.查找的数比当前位置数小（即能构成上升序列）
                // 2.查找的数的最大上升子序列长度加一 比目前所记录的最大上升子序列长度大
                if (nums[j] < nums[i] && (result[j] + 1) > result[i]) {
                    result[i] = result[j] + 1;
                }
            }
            // 记录从开始到当前位置所求最大上升子序列长度最大的
            max = Math.max(result[i], max);
        }

        return max;
    }
}
```  

方法二： 二分法+贪心
（官方题解称之为dp，我个人倾向于贪心）

根据题目中的序列进行举例分析nums = [10,9,2,5,3,7,101,18]，最大上升子序列为[2,3,7,101]，长度为4  
思路： 以未知的最长子序列为对象进行分析。  
假设已知nums序列的一个上升子序列为x[0...n]。题目是求x[0...n]的最大长度，因此所有的行为都以能够提高长度为目的。若现在将nums[i]插入x[0...n]中，判断其对增长上升子序列长度的影响。  
* 若nums[i]>x[n]，则插入nums[i]能够增加上升子序列长度；
* 若nums[i]<x[n]，则将nums[i]替换x[n]中的值不能够增加上升子序列长度，但是，能够提高x[0...n]长度增加的**可能性**（贪心）。实际上，要想上升子序列的值增加只有两种可能性：
1. 替换为nums[i]使得x[0...n]在后续的插入中，出现比当前子序列长度更长的上升子序列。
2. 有比x[n]更大的数在后面的时候会插入进来。
所以为了保留两种可能性，替换位置选择比nums[i]大且最近的位置。即，若x[m1]<nums[i]<x[m2]，则替换x[m2]（采用极端情况理解，令m2=n）。  
由上的分析可知算法思路：  
选择一个nums[i]，查找nums[i]应该插入到x[0...n]这个子序列的位置（采用二分法查找）。若在最后，则直接插入（第2中可能性）;若在中间则替换比本身大的（第1种可能性）。

```java  
public class Solution {
    // 二分法
    public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];// 假设整个都是最长子序列
        int length = 0; // 存储最长子序列长度的

        for (int num : nums) {
            // Arrays.binarySearch()查找，若不存在则返回负数，数值为索引位置加1
            // 查找改nums[i]所处位置
            int index = Arrays.binarySearch(dp, 0, length, num);

            if (index < 0) { // 没找到，替换比它大的
                index = -index-1;
                dp[index] = num;
            }

            if (index == length) { //发现在最后一个
                length++; // 最长子序列长度加一
                dp[index] = num; // 把这个数值插入最后
            }
        }

        return length;
    }
}
```  

---

**参考资料*
* 网友回答：  
[https://blog.csdn.net/yopilipala/article/details/60468359](https://blog.csdn.net/yopilipala/article/details/60468359)
* 官方英文题解：  
[https://leetcode.com/articles/longest-increasing-subsequence/](https://leetcode.com/articles/longest-increasing-subsequence/)  
