367. 有效的完全平方数
https://leetcode-cn.com/problems/valid-perfect-square/

思路：完全平方数是0，1，4，9，16...之间有个规律每两个数之间的差为1，3，5，7，9。。。一个等差数列。 所以一个数如果为完全平方数的话，
就会满足这个规律。
参考资料里面还有另外两种思路，都可以看下。

class Solution {
   public boolean isPerfectSquare(int num) {
     int i = 1;      //从1开始减
     while (num > 0) {
         num -= i;   
         i += 2;    //按规律增加
     }
     return num == 0;  //如果等于0，说明num满足这个规律
 }
}


参考：
https://leetcode.com/problems/valid-perfect-square/discuss/83874/A-square-number-is-1%2B3%2B5%2B7%2B...-JAVA-code
