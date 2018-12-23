**最佳时机买卖股票2**
----
https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/

### 思路一
题目的意思是不限买卖股票的次数，并且每次买股票的时候必须在上一股卖出之后，在此基础上求出最大利润。  
我们只需求出所有的有序段的差值之和即可。遍历数组，找到第一个高峰，然后计算高峰与低谷的差值，更新新的低谷，然后再继续寻找下一个高峰，累加差值即可。  
**你必然有这样的疑问，我找到高峰之后，可能后面还有更高的峰，我为什么非的此时再卖而不是在更高的峰卖呢？这样真的能达到最大值吗**？

你这样想，你当前的高峰后面是另一个低谷，这个差值就是你分开买比一次性买到最高峰的多出来的部分啊，也就是重叠部分。如果你直接从低谷买到最高峰，那么这个差值你是赚不到。而你每一个低谷到第一个高峰都买卖啊，这个额外的差值你都赚到了。  

![image](https://leetcode.com/media/original_images/122_maxprofit_1.PNG)
A + B > C，对吧
```java
    public int maxProfit1(int[] prices) {
        int result = 0;
        int low = 0;
        int len = prices.length;
        for (int i = 0; i < len; i++) {
            //寻找当前碰到第一个高峰
            if (i + 1 == len || prices[i] >= prices[i + 1]) {
                result += prices[i] - prices[low];
                //更新低谷
                low = i + 1;
            }
        }
        return result;
    }
```

### 思路二
还是看上图，既然我们求的是所有的有序段的差值和，那么我们也可以不必每次都求出这个段区间后再求和。而是从小事做起，只要第二天比第一天高，我们就买卖。意味着我们不用考虑具体的有序段是什么，只需关注当前是有序，我们就累加利润即可。类似爬坡的过程，通过累加求出上图的中A的利润。

```java
    /**
     * 一阶段
     * 累加所有的增值即可
     * @param prices
     * @return
     */
    public int maxProfit2(int[] prices) {
        int result = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1]) {
                result += prices[i] - prices[i - 1];
            }
        }
        return result;
    }
```
参考：
- https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/solution/