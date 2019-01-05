**231. 2的幂**  
---

[https://leetcode-cn.com/problems/power-of-two/](https://leetcode-cn.com/problems/power-of-two/)  
思路:
解法一:
直接判断当前这个数字是否等于1,如果等于1则当前是2的0次幂,其次判断当前的数字取模
能否除尽,如果不能直接返回,如果能就继续计算
```java  
  private boolean is2reverse(int n) {
        if (n == 1) {
            return true;
        }
        if (n >= 2 && n % 2 == 0) {
            return is2reverse(n / 2);
        }
        return false;
  }
  
```
**复杂度分析**  
时间复杂度:O(N)
空间复杂度:O(1)
---  
解法二:
2的次幂,意味着n&(n-1)的值为0,如果不是2的次幂那么返回值不是0了
```java  
  private boolean is2reverse(int n) {
        if (n < 0) {
            return false;
        }
        int x = n & (n - 1);
        return x == 0;
  }
``` 

**复杂度分析**  
时间复杂度:O(1)
空间复杂度:O(1)
---
**参考资料**  

* 网友高票Java解法：  
[https://blog.csdn.net/chenchaofuck1/article/details/51226899](https://blog.csdn.net/chenchaofuck1/article/details/51226899)  
