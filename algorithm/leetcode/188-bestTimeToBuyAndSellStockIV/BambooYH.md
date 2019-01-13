**买卖股票的最佳时机IV**  
[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-iv/)  
方法一：**动态规划**  
**思路**  
这个题是一道明显的动态规划题，跟前面几道类似的题有区别，要求最多可以进行K次交易，当然少于K次也是可以的。首先我们可以注意到一种特殊情况，就是`K > Len(array)`，在这种情况下，其实就相当于可以进行任意次交易，因为不管进行多少次交易，一定不会超过K次。  
现在来考虑普通情况，我们用dp[i][j]表示，截止到第j天，最多i次交易所获得的最大利润。dp[i][j]的大小跟两个因素有关。
`dp[i][j] = Max(dp[i][j-1],prices[j]-prices[m] + dp[i-1][m-1])(m > 0 && m <= j-1)`.也就是`dp[i][j] = Max(dp[i][j-1],prices[j] + max(dp[i-1][m-1] - prices[m]))`.在第一个式子中，我们可以看到m是一个范围，所以我们在第二个式子中，用max来代表m不同取值的情况。首先`dp[i][j]`跟`dp[i][j-1]`有关,有可能到第j-1天，第i次交易已经取得了最大利润，后面的天数都不会超过这个利润。`dp[i][j]`还跟`dp[i-1][m-1]`有关，也就是说，到第m-1天为止，只进行了i-1次交易，第i次交易就是`prices[j]-prices[m]`.我们要做的就是找到这个最大值。  
**算法**  
先处理一下特殊情况，即`K > Len(array)`的情况。然后交易次数i从1开始，一直到K。对于每次交易，从第一天开始，一直遍历到最后一天，从而得到dp[i][j]的最大值。

**代码**
```
 public int maxProfit(int k, int[] prices) {
        int len = prices.length;
        //如果K大于数组长度的一半，就相当于可以进行任意次交易，这种情况下，只要数组局部上升，差就是我们的利润。
        if (k >= len / 2) return quickSolve(prices);
        //初始化数组，i表示第i次交易，j表示进行到第j天为止
        int[][] t = new int[k + 1][len];
        for (int i = 1; i <= k; i++) {
            //临时的利润最大值，其实这个就是上面思路部分所说的dp[i-1][m-1] - prices[m],通过tmpMax，我们可以在遍历j的过程中，求出dp[i-1][m-1] - prices[m]的最大值
            int tmpMax =  -prices[0];
            //从1开始遍历，求截止到第j天，最多i次交易能获得的最大利润。
            for (int j = 1; j < len; j++) {
                //参考思路部分
                t[i][j] = Math.max(t[i][j - 1], prices[j] + tmpMax);
                tmpMax =  Math.max(tmpMax, t[i - 1][j - 1] - prices[j]);
            }
        }
        return t[k][len - 1];
    }
    

    private int quickSolve(int[] prices) {
        int len = prices.length, profit = 0;
        for (int i = 1; i < len; i++)
            //如果第i天的价格大于第i-1天的价格，就是我们可以得到的利润。
            if (prices[i] > prices[i - 1]) profit += prices[i] - prices[i - 1];
        return profit;
    }
```
复杂度分析：  
假设数组的长度为N   
空间复杂度: O(KN)  
时间复杂度：·O(KN)