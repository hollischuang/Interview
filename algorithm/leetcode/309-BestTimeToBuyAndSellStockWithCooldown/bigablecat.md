**309. 最佳买卖股票时机含冷冻期**  
---
[https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/)  

* 网友高票Java答案(动态规划)  

```java  
    public static int maxProfit(int[] prices) {
        //定义变量
        //sell表示只能卖出或持有时所得的总利润
        //prev_sell用于缓存上一次的sell值
        //buy表示只能买入或持有时所得的总利润
        //prev_buy用于缓存上一次的buy值
        int sell = 0, prev_sell = 0, buy = Integer.MIN_VALUE, prev_buy;  

        for (int price : prices) {
            // buy的值来自上一次循环结果，将其赋值给prev_buy
            prev_buy = buy;

            // 如果当前日期只能买入或持有，buy的值有两种情况
            // 第一种情况：在当前日期进行了买入操作
            // prev_sell - price
            // prev_sell 是前一次进行卖出操作时所得利润总和
            // price是当前日期价格，也就买入股票所需的支出
            // 利润-支出，得到在当前日期买入后剩余的利润
            // 第二种情况：当前日期不进行任何操作
            // 直接使用pre_buy的值即可
            // 从二者中比较出利润较大者，赋值给变量buy
            // buy就是当前日期只能买入或持有时所得最大利润
            buy = Math.max(prev_sell - price, prev_buy);

            // 同理可得只能卖出或持有时所得利润sell的值
            prev_sell = sell;
            
			// 不同的是对的Math.max(prev_buy + price, prev_sell)部分的解释
            // prev_buy + price
            // prev_buy 是前一次进行买入操作时所得利润总和
            // price是当前日期价格，也就卖出股票所得的收入
            // 利润+支出，得到在当前日期卖出后总共获得的利润
            sell = Math.max(prev_buy + price, prev_sell);
        }
		
        //到最后一个交易日为止，最后的一次操作只能是卖出才能清仓
        //所以返回sell的值即可
        return sell;
    }

```  

**复杂度分析**  

时间复杂度：O(n)，遍历一次

空间复杂度：O(1)，没有使用额外空间

---

**参考资料**  

* 网友高票Java答案：  
[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/75927/Share-my-thinking-process](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/75927/Share-my-thinking-process)  
