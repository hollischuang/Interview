**954. 二倍数对数组**  
---

[https://leetcode-cn.com/problems/array-of-doubled-pairs/](https://leetcode-cn.com/problems/array-of-doubled-pairs/)  

* 官方题解(英文)  

```java  
    public static boolean canReorderDoubled(int[] A) {
        //定义一个HashMap对象count，用于统计每个元素在数组中出现的次数
        Map<Integer, Integer> count = new HashMap();
        //遍历数组A
        for (int x : A)
            //count.getOrDefault(x, 0) 在count中查找key为x的value
            //如果不存在x，说明在count中只出现过一次，value用默认值0
            //取出的value就是元素x的数值在数组A中出现的次数
            //+1计数每次递增
            count.put(x, count.getOrDefault(x, 0) + 1);

        // B = A as Integer[], sorted by absolute value
        //定义一个与数组A等长的数组B
        Integer[] B = new Integer[A.length];
        //遍历数组A
        for (int i = 0; i < A.length; ++i)
            //将数组A中的元素依次赋予数组B
            B[i] = A[i];
        //完成for循环后，B是数组A的一份拷贝
        //Arrays.sort使用的DualPivotQuickSort在经典快排基础上改进，时间复杂度稳定为O(nlogn)
        //第二个参数Comparator.comparingInt(Math::abs)是一个实现了Comparator接口的对象
        //Math::abs是lambda表达式的写法，不使用lambda的等价写法如下
        /**
         * Comparator.comparingInt(new ToIntFunction<Integer>() {
         *     @Override
         *     public int applyAsInt(Integer value) {
         *         return Math.abs(value);
         *     }
         * });
         */
        //在ToIntFunction接口的applyAsInt方法中调用了Math.abs对数组B中的每一个元素取绝对值
        //将数组B的元素根据绝对值大小排序
        Arrays.sort(B, Comparator.comparingInt(Math::abs));

        //遍历数组B中的元素
        for (int x : B) {
            //count.get(x) == 0 乍看令人困惑，实际上需要结合后续代码来理解
            //count.get(x)获取了元素x在count中的计数
            //后续代码对符合题设条件的x进行了计数递减的操作后放回了count
            //所以如果出现count.get(x)==0的情况，说明符合条件的x计数已经归零
            //continue继续循环
            if (count.get(x) == 0) continue;
            //count.getOrDefault(2 * x, 0)查找2*x的计数
            //如果2*x的计数<= 0，说明数组B中不存在2*x
            //数组中没有x的二倍数，不符合题设，返回false
            if (count.getOrDefault(2 * x, 0) <= 0) return false;

            //运行到此处，说明数组中存在x的二倍数2*x
            //分别将x和2*x在count中的计数递减一次
            count.put(x, count.get(x) - 1);
            count.put(2 * x, count.get(2 * x) - 1);
        }
        // 所有元素的计数都消除为0
        // 说明数组正好可以按照题设将元素两两结合分配为二倍数对
        return true;
    }

```  

**复杂度分析**  

时间复杂度：O(nlogn)，
方法中对数组进行了三次遍历，时间复杂度为3n，
同时使用了一次Arrays.sort为数组排序，
Arrays.sort使用的DualPivotQuickSort在经典快排基础上改进，
时间复杂度稳定为O(nlogn)，
总的时间复杂度为nlogn+3n，消去低阶项，最终时间复杂度为O(nlogn)

空间复杂度：O(n)，
Arrays.sort排序方法的空间复杂度是O(n)，
使用了HashMap存储数组中所有对象，空间复杂度是O(n)，
最终的空间复杂度是O(n)  

---

**参考资料**  

* 英文官方题解：  
[https://leetcode.com/problems/array-of-doubled-pairs/solution/](https://leetcode.com/problems/array-of-doubled-pairs/solution/)  
