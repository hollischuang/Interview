**122. 买卖股票的最佳时机 II**  
---
[https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock-ii/)  

* 官方题解1：暴力法  

```java  
	
    public int maxProfit(int[] prices) {
        return calculate(prices, 0);
    }

    public int calculate(int prices[], int s) {
        //s是遍历起始的下标，如果超过数组长度直接返回0
        if (s >= prices.length)
            return 0;
        //定义一个最大值缓存max
        int max = 0;
        //外循环从起始位置起遍历元素
        for (int start = s; start < prices.length; start++) {
            //定义最大利润的缓存maxprofit
            int maxprofit = 0;
            //i = start + 1，内循环从起始位置的第二天开始
            for (int i = start + 1; i < prices.length; i++) {
                //如果当前价格大于起始位置的价格
                if (prices[start] < prices[i]) {
                    //prices[i] - prices[start]得到在start天买入，第i天卖出所获利润
                    //因为在第i天卖出了，所以尝试在第i+1天买入
                    //calculate(prices, i + 1)递归调用，得到在i+1天开始持有所能获得的最大利润
                    //经过递归，profit获得的就是在第i天卖出到最后一天为止所能获得的最大利润
                    int profit = calculate(prices, i + 1) + prices[i] - prices[start];
                    //如果利润profit大于内循环已知的最大利润
                    if (profit > maxprofit)
                        //将利润profit赋值给内循环最大利润maxprofit
                        maxprofit = profit;
                }
            }
            //如果最大利润maxprofit大于外循环最大利润缓存max
            if (maxprofit > max)
                //将最大利润maxprofit赋值给max
                max = maxprofit;
        }
        //最终返回以s天为起点的交易组合中最大利润
        return max;
    }

```  

**复杂度分析**  

时间复杂度：O(n^n)，
共调用递归n^n次

空间复杂度：O(n)，
每递归一次占用O(1)的空间复杂度，
递归函数的调用在内循环中，最大深度是n，
所以空间复杂度是O(n)

---

* 官方题解2：峰谷法  

```java  
	
    public int maxProfit(int[] prices) {
        //定义数组的起始位置
        int i = 0;
        //定义一个谷值valley
        int valley = prices[0];
        //定义一个峰值peak
        int peak = prices[0];
        //定义一个最大利润
        int maxprofit = 0;
        //从头遍历数组
        while (i < prices.length - 1) {
            //prices[i] >= prices[i + 1] 只要当天的价格大于或等于次日的价格，就一直递增
            while (i < prices.length - 1 && prices[i] >= prices[i + 1])
                i++;
            //当prices中元素不满足prices[i] >= prices[i + 1]时，即prices[i] < prices[i + 1]
            //说明prices[i]是最近的谷底
            valley = prices[i];
            //继续遍历，用同样的手法找到峰值
            while (i < prices.length - 1 && prices[i] <= prices[i + 1])
                i++;
            peak = prices[i];
            //最大利润是峰值和谷底的差值
            //+=将所有差值不断累加，得到了最大利润总和
            maxprofit += peak - valley;
        }
        //返回最大利润
        return maxprofit;
    }

```  

**复杂度分析**  

时间复杂度：O(n)，
遍历一次，虽然嵌套了while循环，但是使用同一个控制变量，
最终只遍历了数组一次

空间复杂度：O(1)，
需要常量的空间

---

* 官方题解3：一次遍历法  

```java  
	
    public int maxProfit(int[] prices) {
        //定义一个最大利润变量maxprofit
        int maxprofit = 0;
        //变量价格数组
        for (int i = 1; i < prices.length; i++) {
            //如果当天价格高于前一天价格
            if (prices[i] > prices[i - 1])
                //当天价格减去前一天价格，得到利润
                //maxprofit累加当天所得利润
                maxprofit += prices[i] - prices[i - 1];
        }
        //最终返回的maxprofit是所有利润总和
        return maxprofit;
    }

```  

**复杂度分析**  

时间复杂度：O(n)，
遍历数组一次

空间复杂度：O(1)，
需要常量空间

---

**参考资料**  

* 官方题解：  
[https://leetcode-cn.com/articles/best-time-to-buy-and-sell-stock-ii/](https://leetcode-cn.com/articles/best-time-to-buy-and-sell-stock-ii/)  