**123.买卖股票的最佳时机 III**
---
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/

### 思路一
这个题是典型的动态规划问题，这里题目虽然说的要求最多购买2次，那么我们可以拓展一下，提出最多购买k次的最大值。  
主要有以下状态转移方程：
```math
dp[k][i] = max(dp[k][i - 1], dp[k - 1][j] + prices[i] - prices[j] ( j in [0, i - 1]))

dp[k][i] = max(dp[k][i - 1], prices[i] + max(dp[k - 1][j]  - prices[j] ( j in [0, i - 1])))
```
dp[k][i]表示第k次交易截止到第i天最大的收益，它等于第k次交易截止到第i - 1天最大的收益和第k次交易截止到第j天并且我在第j天重新买了一张，第i天卖掉的最大值。  
注意到上式中`max(dp[k - 1][j]  - prices[j] ( j in [0, i - 1]))`会重复计算，所以这里我们只需在遍历时维护一个关于dp[k - 1][j]  - prices[j] ( j in [0, i - 1])的最大值即可。

```java
    /**
     *
     * dp[k][i] = max(dp[k][i - 1], dp[k - 1][j] + prices[i] - prices[j] ( j in [0, i - 1]))
     *          = max(dp[k][i - 1], prices[i] + max(dp[k - 1][j] - prices[j])
     * @param prices
     * @return
     */
    public int maxProfit4(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int totalK = 2;
        int[][] dp = new int[totalK + 1][prices.length];
        for (int k = 1; k <= totalK; k++) {
            int maxProfit = - Integer.MIN_VALUE;
            for (int i = 1; i < prices.length; i++) {
                maxProfit = Math.max(maxProfit, dp[k - 1][i - 1] - prices[i - 1]);
                dp[k][i] = Math.max(dp[k][i - 1], prices[i] + maxProfit);
            }
        }
        return dp[totalK][prices.length - 1];
    }
```
### 思路二
思路二的状态方程引入冗余，为了之后的简化：
```math
dp[k][i] = max(dp[k][i - 1], dp[k - 1][j - 1] + prices[i] - prices[j] ( j in [0, i]))

dp[k][i]= max(dp[k][i - 1], prices[i] + max(dp[k - 1][j - 1] - prices[j]) ( j in [0, i])
```
其中j可以取到i，意思是我们当天买，当天卖，引入这样的情况只不过是为了后续简化方便，这里我们把K放到了内循环，所以要用max来存放k不同时维护的最大值。
```java
    public int maxProfit5(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int totalK = 2;
        int[][] dp = new int[totalK + 1][prices.length];
        int[] max = new int[totalK + 1];
        Arrays.fill(max, - prices[0]);
        for (int i = 1; i < prices.length; i++) {
            for (int k = 1; k <= totalK; k++) {
                max[k] = Math.max(max[k], dp[k - 1][i - 1] - prices[i]);
                dp[k][i] = Math.max(dp[k][i - 1], prices[i] + max[k]);
            }
        }
        return dp[totalK][prices.length - 1];
    }
```

又因为dp只依赖i - 1，所以可以压缩状态为：
```java
    public int maxProfit6(int[] prices) {
        if (prices == null || prices.length == 0) {
            return 0;
        }
        int totalK = 2;
        int[] dp = new int[totalK + 1];
        int[] max = new int[totalK + 1];
        Arrays.fill(max, Integer.MIN_VALUE);
        for (int i = 0; i < prices.length; i++) {
            for (int k = 1; k <= totalK; k++) {
            //注意这里为什么可以直接dp[k - 1] - prices[i]呢？
                max[k] = Math.max(max[k], dp[k - 1] - prices[i]);
                dp[k] = Math.max(dp[k], prices[i] + max[k]);
            }
        }
        return dp[totalK];
    }
```
答：当k = 1, dp[k - 1] = 0, 对应的第一次买；当k = 2, dp[k - 1] 其实是dp[1][i]，也就说我第一次买，截止到i天取得最大值，虽然这里减去- prices[i]，后面会prices[i]抵消掉。这就是思路二的独特，思路一是无法这么化简的。

展开：
```
    public int maxProfit3(int[] prices) {
        int buy1 = Integer.MIN_VALUE;
        int buy2 = Integer.MIN_VALUE;
        int sell1 = 0;
        int sell2 = 0;
        for (int price : prices) {
            buy1 = Math.max(buy1, - price);
            sell1 = Math.max(sell1, price + buy1);
            buy2 = Math.max(buy2, sell1 - price);
            sell2 = Math.max(sell2, buy2 + price);
        }
        return sell2;
    }
```

参考：https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iii/discuss/135704/Detail-explanation-of-DP-solution