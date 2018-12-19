**455. 分发饼干**  
---

[https://leetcode-cn.com/problems/assign-cookies/](https://leetcode-cn.com/problems/assign-cookies/)  

* 网友高票Java解法  

```java  

    public int findContentChildren(int[] g, int[] s) {
        //调用java.util.Arrays.sort排序方法
        //分别给孩子期望和饼干大小排序
        Arrays.sort(g);
        Arrays.sort(s);
        int i = 0; //定义数组g的下标初始值i
        //遍历数组s
        for (int j = 0; i < g.length && j < s.length; j++) {
            //g[i]是数组g在i位置的元素，表示第i个孩子的胃口
            //s[j]是数组s在j位置的元素，表示第j个饼干的尺寸
            //经过排序，g[i]从孩子最小的胃口开始
            //g[i]<=s[j]说明j位置的饼干可以满足第i个孩子
            //此时让i++，看s[j]是否能满足更大胃口的孩子
            if (g[i] <= s[j]) i++;
        }
        //最后返回的i就是最多能满足多少个孩子
        return i;
    }

```  

**复杂度分析**  

时间复杂度：O(nlogn)，
设两个数组的长度分别是 m 和 n
Arrays.sort使用的DualPivotQuickSort在经典快排基础上改进，
时间复杂度稳定为O(nlogn)，
Arrays.sort使用了两次，所以排序的时间复杂度是
mlogm + nlogn，
for循环内虽然对两个数组进行操作，
但是两个数组都没有被重复从头遍历，
所以最坏情况的遍历次数是两个数组的长度之和
m+n，
最终的时间复杂度是
O(mlogm + nlogn + m + n) = O(nlogn)

空间复杂度：O(n)，
Arrays.sort排序方法的空间复杂度是O(n)，
使用了两次，所以空间复杂度是O(2n)，
最终的空间复杂度是O(n)  

---

**参考资料**  

* 网友高票Java解法：  
[https://leetcode.com/problems/assign-cookies/discuss/93987/Simple-Greedy-Java-Solution](https://leetcode.com/problems/assign-cookies/discuss/93987/Simple-Greedy-Java-Solution)  
