**714. 买卖股票的最佳时机含手续费**  
---
[https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-transaction-fee/)  

*  选择的关键是找到一个最大后是不是能够卖掉stock,重新开始寻找买入机会。
比如序列1 3 2 8,如果发现2小于3就完成交易买1卖3,此时由于fee=2,（3-1-fee）+(8-2-fee)<(8-1-fee),
所以说明卖早了,令max是当前最大price,当（max-price[i]>=fee）时可以在max处卖出,且不会存在卖早的情况,
再从i开始重新寻找买入机会
贪心解法:
```java  
public class Solution {
     public static int maxProfit(int[] prices, int fee) {
            int n = prices.length;
            if (n <= 1) {
                return 0;
            }
            int p = 0, curP = 0;
            int minP = prices[0], maxP = prices[0];
            for (int i = 1; i < n; i++) {
                minP = Math.min(minP, prices[i]);
                maxP = Math.max(maxP, prices[i]);
                curP = Math.max(curP, prices[i] - minP - fee);
                if (maxP - prices[i] >= fee) {
                    p += curP;
                    curP = 0;
                    maxP = prices[i];
                    minP = prices[i];
                }
            }
            return p + curP;
    }
}
```  
* 动态转移点：手上有没有股票 进行DP。对于第i天的最大收益,应分成两种情况,一是该天结束后手里没有stock,
可能是保持前一天的状态也可能是今天卖出了,此时令收益为cash；二是该天结束后手中有一个stock,
可能是保持前一天的状态,也可能是今天买入了。由于第i天的情况只和i-1天有关,所以用两个变量cash和buy就可以,
不需要用数组

```java 
class Solution {
    public int maxProfit(int[] prices, int fee) {
        int n=prices.length;
        if(n<=1)
            return 0;
        int buy=-prices[0];
        int cash=0;
        for(int i=1;i<n;i++){
            cash=Math.max(cash,buy+prices[i]-fee);
            buy=Math.max(buy,cash-prices[i]);
        }
        return cash;
    }
}
```

**参考资料**  
[https://blog.csdn.net/zw159357/article/details/82260077](https://blog.csdn.net/zw159357/article/details/82260077)  
