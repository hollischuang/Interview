69. X的平方根
	https://leetcode-cn.com/problems/sqrtx/
	
	求一个非负整数的平方根
	思路1：最简单的想法，从0开始遍历，如果a的平方数小于或者等于目标值，同时a+1的平方数大于目标值，则a为所求值。但是这个方法算法复杂度太大，
	当目标值为Integer_Max的时候，效率太低。
	思路2：二分法
	
	class Solution {
	    public int mySqrt(int x) {
	        int left = 1;  //这里设置成1 没有设置为0 主要是考虑x可能为0。
	        int right = x;
	        int mid = 0;
	        while(left <=right){
	            mid = left + (right-left)/2;    
	            if(mid<=x/mid && mid+1>x/(mid+1)) // mid<=x/mid 也就是mid*mid<=x 但是mid的平方有可能数值过大
	                return mid; 
	            if(mid > x/mid)     // 从左半边开始找
	                right = mid-1;
	            else                //从右半边开始找
	                left =mid+1;
	        }
	        return 0;
	    }
	
	}
	
	总结：个人感觉 在一个区间内 找特定的一个数的时候 都优先考虑二分法
	
	参考资料：
	https://leetcode.com/problems/sqrtx/discuss/25047/A-Binary-Search-Solution
