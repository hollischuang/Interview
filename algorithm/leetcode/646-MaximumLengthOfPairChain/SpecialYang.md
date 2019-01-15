**646.最长数对链**
---
https://leetcode.com/problems/maximum-length-of-pair-chain/
如果你把一对数捏成一个数，这不就是最长上升子序列问题吗？
### 思路一
动态规划问题，我们首先按照第1个数的大小排序所有的数对，然后有如下状态转移方程：
```math
dp[i] = max(dp[i], dp[j] + 1) (j \in [0, i))
```
dp[i]表示以第i个数对结尾的最大数对链长度，那么dp[i]的值为dp[i]自身（初始为1）与 从第j个数对连接到i数对的最大值，可以联想最长上升子序列问题哈。
```java
    /**
     * 最长递增子序列问题的变形
     *
     * @param pairs
     * @return
     */
    public int findLongestChain1(int[][] pairs) {
        Arrays.sort(pairs, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[0] - o2[0];
            }
        });
        int[] dp = new int[pairs.length + 1];
        int max = 0;
        for (int i = 0; i < pairs.length; i++) {
            dp[i] = 1;
            for (int j = 0; j < i; j++) {
                if (pairs[j][1] < pairs[i][0]) {
                    dp[i] = Math.max(dp[i], dp[j] + 1);
                }
            }
            max = Math.max(max, dp[i]);
        }
        return max;
    }
```
#### 复杂度
- 时间复杂度：O(n^2)
- 空间复杂度：O(n)

### 思路二
按第二个数排序，这样我们优先安排第二个数最小的，因为安排当前第二个数最小比安排当前不是第二个数最小要好。  
假设全局最优的链中第i个的第二个数不是当时安排的最小，那么必然这里可以替换成最小，最终只会导致这个全局长度不变或者增大，所以最好的选择就是每次都安排最小的。
```java
    /**
     * 贪心
     * 以第二数排序
     * 优先添加末尾数小的，这样可以给后面的pair更大的选择
     * @param pairs
     * @return
     */
    public int findLongestChain2(int[][] pairs) {
        Arrays.sort(pairs, new Comparator<int[]>() {
            @Override
            public int compare(int[] o1, int[] o2) {
                return o1[1] - o2[1];
            }
        });
        int curEnd = Integer.MIN_VALUE;
        int max = 0;
        for (int[] pair : pairs) {
            if (curEnd < pair[0]) {
                curEnd = pair[1];
                max++;
            }
        }
        return max;
    }
```
#### 复杂度
- 时间复杂度：O(nlogn)
- 空间复杂度：O(1)
以上两者都依赖你所选的排序算法。