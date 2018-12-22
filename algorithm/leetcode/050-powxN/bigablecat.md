**50. Pow(x, n)**  
---
[https://leetcode-cn.com/problems/powx-n/](https://leetcode-cn.com/problems/powx-n/)  


* 网友高票Java解法，递归分治  

```java  

    public double myPow(double x, int n) {
        //如果n==0，返回1，因为x的0次方为1
        if (n == 0)
            return 1;
        if (n < 0) {
            //因为 n = -1*(-n)，所以x的n次方等于x的-1次方的-n次方
            //所以n等于负数时进行如下两步操作
            // n = -n让n变为正
            n = -n;
            //让x成为x的倒数
            x = 1 / x;
            //判断原来的n值是否超出了JavaInteger的下界
            if (-n == Integer.MIN_VALUE) {
                //考虑到Java语言中Integer的取值范围在-2147483648到2147483647之间
                //Integer.MIN_VALUE = -2147483648，
                //Integer.MAX_VALUE = 2147483647，
                //当n的值在取值范围之外，编译无法通过，不予考虑
                //当 n = -2147483648 时，让 n = -n 得到 n = 2147483648 超过了Integer.MAX_VALUE=2147483647
                //为了避免这种情况，在n = -n = 2147483648后
                //让n-1 = 2147483647，同时取出一个x，按照奇数的计算方式返回结果
                return x * myPow(x, (n - 1));
            }
        }
        //接下来进行分治，将求x的n次方转变为求x平方的(n/2)次方
        //判断n是否为偶数，如果是偶数，只需递归调用myPow，如果是奇数，取出一个x，再与myPow结果相乘
        return (n % 2 == 0) ? myPow(x * x, n / 2) : x * myPow(x * x, n / 2);
    }


```  

**复杂度分析**  

时间复杂度：O(logn)，
每次递归，n就被2分一次，n/2/2...，
所以总共调用递归方法的次数是logn次，
递归方法中没有循环，只有常数级的操作，
所以总的时间复杂度是O(logn)

空间复杂度：O(logn)，
每次递归都占用O(1)的空间

---

**参考资料**  

* 网友高票Java解法：  
[https://leetcode.com/problems/powx-n/discuss/19546/Short-and-easy-to-understand-solution](https://leetcode.com/problems/powx-n/discuss/19546/Short-and-easy-to-understand-solution)  
