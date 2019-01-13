**198. 打家劫舍**  
---
[https://leetcode-cn.com/problems/house-robber/](https://leetcode-cn.com/problems/house-robber/)  
  
**思路**  
你是一个专业的小偷,计划偷窃沿街的房屋。每间房内都藏有一定的现金,影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统,
如果两间相邻的房屋在同一晚上被小偷闯入,系统会自动报警。给定一个代表每个房屋存放金额的非负整数数组,计算你在不触动警报装置的情况下,
能够偷窃到的最高金额。

1、首先想一想如果是暴力如何做？

假设从最后一家店铺开始抢,那么只会遇到2种情况,即：抢这家店和下下家店,或者不抢这家店。
所以我们得到递归的公式:
Math.max(solve(nums,index-1),solve(nums,index-2)+nums[index]);

2、上面的暴力算法虽然能够得到正确的结果,但是显然递归的效率是很低的,如果有n家店铺,每家店铺有2种可能,那么时间复杂度就是2的n次方。那么如何优化呢？

我们分析一下：
如果我们开始抢的是第n-1家店,那么后面可以是（n-3,n-4,n-5,n-6....）;
如果我们开始抢的是第n-2家店,那么后面可以是（n-4,n-5,n-6,....）;
那么这两种情况显然n-3之后的n-4,n-5,n-6,....都重复计算了。显然这里有非常大的优化空间。通常我们使用空间来换时间,即用一个数组记录每次计算的结果,
这样每次情况只需要计算一次,再次遇到只需直接返回结果即可,大大优化了时间


```java  
   class Solution {    
       public static int[] result;    
       public int solve(int[] nums,int index){
           if(index < 0){
                return 0;
           }      
           if(result[index] >= 0){
               return result[index];
           }
           result[index]=Math.max(solve(nums,index-1),solve(nums,index-2)+nums[index]);
           return result[index];        
       }    
       public int rob(int[] nums) {
           result = new int[nums.length];
           for(int i=0;i<nums.length;i++){
               result[i]=-1;
           }
           return solve(nums,nums.length-1);        
       }
   }
```  

**参考资料**  
[https://www.jianshu.com/p/77f1e8157277](https://www.jianshu.com/p/77f1e8157277)  
