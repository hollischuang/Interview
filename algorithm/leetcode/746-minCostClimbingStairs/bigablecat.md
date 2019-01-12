**746. 使用最小花费爬楼梯**  
---
[https://leetcode-cn.com/problems/min-cost-climbing-stairs/](https://leetcode-cn.com/problems/min-cost-climbing-stairs/)  

```java  

    /**
     * https://leetcode.com/articles/min-cost-climbing-stairs/
     * <p>
     * 英文官方题解
     *
     * @param cost
     * @return
     */
    public static int minCostClimbingStairs(int[] cost) {
        //根据题意，爬楼可以走一个台阶或者两个台阶
        //定义两个变量f1，f2分别记录走一个台阶和两个台阶的花费
        int f1 = 0, f2 = 0;
        //从尾向头方向遍历数组元素
        for (int i = cost.length - 1; i >= 0; --i) {
            //cost[i]获取在第i个阶梯时的花费
            //根据题意，有走一个台阶和两个台阶两种走法
            //f1表示走到第i+1个台阶的花费
            //f2表示走到第i+2个台阶的花费
            //Math.min(f1, f2);从两种走法中获取花费较小的一种
            //f0得到从第i个阶梯继续向上走到楼顶的所有花费
            //从最后一个台阶开始计算时
            //不存在第i+1和第i+2个台阶，f1和f2初始值为0，对结果没有影响
            int f0 = cost[i] + Math.min(f1, f2);
            //下一轮将计算第i-1个台阶到楼顶的花费
            //在本轮中，
            //f0、f1、f2分别代表第i个、第i+1个和第i+2个台阶到楼顶的费用
            //那么下一轮，计算第i-1个台阶到楼顶的费用时，需要知道第i个，第i+1个台阶的费用
            //为下一轮的备选答案赋值
            //f1为第i+1个台阶到楼顶的花费，赋值给f2，即为下一轮走2个台阶方案的花费
            f2 = f1;
            //f0位第i个台阶到楼顶的花费，赋值给f1，即为下一轮走1个台阶方案的花费
            f1 = f0;
        }
        //遍历完数组时
        // f1为从cost[0]开始向上到楼顶的花费
        // f2为从cost[1]开始向上到楼顶的花费
        return Math.min(f1, f2);
    }

```  

**复杂度分析**  

时间复杂度：O(n)，
遍历长度为n的数组一次

空间复杂度：O(1)，
f1和f2使用了常数空间

---

**参考资料**  

* 英文官方题解：  
[https://leetcode.com/articles/min-cost-climbing-stairs/](https://leetcode.com/articles/min-cost-climbing-stairs/)  
