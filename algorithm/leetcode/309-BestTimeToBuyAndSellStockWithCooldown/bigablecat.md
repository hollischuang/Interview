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
		
        //上述代码中还有2个问题需要解决
        //第一个问题，会不会在同一天既买入又卖出？
        //如果进行了卖出操作，sell = prev_buy + price
        //prev_buy不是当天的买入的结果
        //所以同一天内，买和卖都是在上一次结果的基础上交易
        //第二个问题，会不会出现连续买入或连续卖出的情况？
        //假设连续买入而没有卖出
        //在第i-1天买入，buy[i-1] = prev_sell - prices[i-1]
        //在第i天继续买入，buy[i] = prev_sell - prices[i]
        //因为没有卖出，所以prev_sell是相等的
        //这样buy[i]的操作实际上冲抵了buy[i-1]的操作
        //即原本在第i-1天用prev_sell买入
        //经过比较，发现第i天买入更划算
        //那么用prev_sell在第i天买入
        //可以看出，只要prev_sell的值保持不变
        //则永远只能在prev_sell的基础上发生1次买入
        //而prev_sell的值发生了变化，说明已经进行了一次卖出操作
        //再一次的买入是在新的卖出基础上进行的
        //同理，sell = prev_buy + price 中sell也只能是一种置换
        //结合冷冻期的解释，可以总结出
        //在同一天内，买和卖都只能基于上一次的交易结果
        //买和卖永远只会成对出现

    }

```  

**复杂度分析**  

时间复杂度：O(n)，遍历一次

空间复杂度：O(1)，没有使用额外空间

---

**参考资料**  

* 网友高票Java答案：  
[https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/75927/Share-my-thinking-process](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-with-cooldown/discuss/75927/Share-my-thinking-process)  
