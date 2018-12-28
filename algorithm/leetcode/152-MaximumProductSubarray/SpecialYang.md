**乘积最大子序列**
---
https://leetcode.com/problems/maximum-product-subarray/

典型的动态规划问题，说实话，第一次做没做出来，菜是原罪，主要卡在了判断当前最大值上面，因为乘积的特性，使得当前的不是最大值可能由于后面的负负得正从而晋升为最大值。  

卡在上面的原因，思维一直停留在和最大子序列那道题。在那道题里，我们判断当前最大值为之前的连续最大和加上当前的数字，或者只有当前的数字，即`dp[i] = max(dp[i - 1] + num[i], num[i])`。若加上当前的值的连续和还没有只有当前值大，说明之前的连续和没有贡献，所以新的子序列要从当前值开始。  

在乘积里就不能这么做了，因为即使乘以当前值的累积没有只有当前值大，这种情路发生在dp[i - 1]为负数，num[i]为正数时。如果仅仅按照最大连续和的做法，就会开启新的序列，前面的dp[i - 1]就会丢弃。这时如果num[i + 1]为负数，那么`dp[i - 1] * num[i] * num[i + 1]` 显然比`num[i] * num[i + 1]`大，所以我们就会丢失这个最大值。  

牛逼的思路就是我们不仅仅要记录以当前位置结尾累积的最大值，还要记录对应的最小值，并且在访问到负数时，之前的最大值与最小值要交换一下，以便之后的正确更新。

记录的最大最小值，我们就可以应对各种情况了，最大值可以在遇到正数时依旧最大，遇到负数可变为最小；最小值在遇到负数翻身别为最大，遇到正数可变为最小。

```java
    /**
     * 维护已i结尾的乘积最大值，最小值
     *
     * 遇到负数，交换最大最小值，因为乘以负数时，最小值会变为最大值
     * @param nums
     * @return
     */
    public int maxProduct2(int[] nums) {
        int result = nums[0];
        for (int i = 1, min = result, max = result; i < nums.length; i++) {
            //遇到负数，交换以i - 1结尾的最大值，最小值
            if (nums[i] < 0) {
                int temp = min;
                min = max;
                max = temp;
            }
            max = Math.max(nums[i], max * nums[i]);
            min = Math.min(nums[i], min * nums[i]);
            result = Math.max(result, max);
        }
        return result;
    }
```
复杂度：
- 时间复杂度：遍历一遍，O(n)
- 空间复杂度：3个变量，O(1)

参考：https://leetcode.com/problems/maximum-product-subarray/discuss/48230/Possibly-simplest-solution-with-O(n)-time-complexity