**954. 二倍数对数组**  
---

[https://leetcode-cn.com/problems/array-of-doubled-pairs/](https://leetcode-cn.com/problems/array-of-doubled-pairs/)  

* 方法1：官方题解(英文)
>排序 + HashMap  

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

* 方法2：递归  
>递归+HashMap  

```java  

    public boolean canReorderDoubled(int[] A) {
        //定义一个HashMap对象count，用于统计每个元素在数组中出现的次数
        Map<Integer, Integer> count = new HashMap();
        //遍历数组A
        for (int x : A)
            //count.getOrDefault(x, 0) 在count中查找key为x的value
            //如果不存在x，说明在count中只出现过一次，value用默认值0
            //取出的value就是元素x的数值在数组A中出现的次数
            //+1计数每次递增
            count.put(x, count.getOrDefault(x, 0) + 1);

        //首先对数组中的0进行处理
        //如果数组中0出现的次数不是偶数，说明0无法两两配成对
        //0不能与其他元素结合成符合题设的数对，所以返回false
        if (count.getOrDefault(0, 0) % 2 != 0) {
            return false;
        } else {
            //如果0出现偶数次，所有0可以成功配对，直接将0的计数消去
            count.put(0, 0);
        }

        //遍历数组A
        for (int x : A) {
            //如果当前元素计数已经消去，继续循环
            if (count.get(x) == 0) continue;
            //将当前元素和存放计数的HashMap对象count传入递归函数findHalfNum
            //findHalfNum的作用是向下溯源，为所有小于等于当前元素值的数字配对
            findHalfNum(x, count);
        }

        //再次遍历数组
        for (int x : A) {
            //如果数组A符合题设，所有元素完成配对，所有计数都应消去为0
            //如果存在计数大于0的元素，说明有不符合题设的元素存在，返回false
            if (count.get(x) > 0) {
                return false;
            }
        }
        //满足所有条件，返回true
        return true;
    }

    /**
     * 递归函数，找到小于等于参数x的所有二倍数进行配对
     * 对完成配对的元素，消去相应的计数
     *
     * @param x
     * @param count
     * @return
     */
    public int findHalfNum(int x, Map<Integer, Integer> count) {
        //x % 2 == 0 如果x不能被2整除，说明x不是其他数字的二倍数，直接返回
        //count.getOrDefault(x / 2, 0) > 0 说明存在一个元素，x是这个元素的二倍数
        if (x % 2 == 0 && count.getOrDefault(x / 2, 0) > 0) {
            //递归调用当前方法，找到能够与x/2配对的更小元素
            if (findHalfNum(x / 2, count) > 0) {
                //count.get(x) - count.get(x / 2)用于判断x和x/2哪个出现的次数更少
                //pairsNum得到的是x和x/2中较少的计数，也就是x和x/2最多能配成几对二倍数
                int pairsNum = (count.get(x) - count.get(x / 2)) < 0 ? (count.get(x)) : count.get(x / 2);
                //分别将x和x/2配对，消去公共计数，比如pairsNum是2，说明x和x/2可以配成2对
                count.put(x, count.get(x) - pairsNum);
                count.put(x / 2, count.get(x / 2) - pairsNum);
            }
        }
        //最后返回x剩余的计数到调用递归的上一级
        //如果x的计数仍有结余，x可以和x*2继续配对
        return count.get(x);
    }

```  

**复杂度分析**  

时间复杂度：O(n)，
方法中对数组进行了三次遍历，时间复杂度为3n，
方法中使用的递归方法取决于数组的长度，时间复杂度为n，
总的时间复杂度为4n，
最终时间复杂度为O(n)

空间复杂度：O(n)
本方法中递归的深度取决于数组的长度n，
所以空间复杂度是O(n)

---

* 方法3：网友高效数组方法  
>整数数组  

```java  

    public static boolean canReorderDoubled(int[] A) {
        //根据题意，-100000 <= A[i] <= 100000
        //分别创建两个整数数组pos和neg
        //pos和neg的长度100001是数组A中可能出现的最大绝对值
        //pos和neg用于计数
        int[] pos = new int[100001];
        int[] neg = new int[100001];
        //遍历数组
        for (int i : A) {
            //如果数组i中的元素大于0，对pos进行操作
            //pos[i]找到pos中第i个元素的值，对其做++递增操作
            //因为数组pos中所有元素的初始值都为0
            //所以pos[i]++实际上是对i进行了计数
            if (i > 0) pos[i]++;
                //同理，当i<0时，取其正值-i，对neg数组进行操作
            else neg[-i]++;
        }
        //上述遍历结束后，pos保存了A中所有正值元素的计数，neg保存了A中所有负值元素的计数
        //分别检查pos和neg中的元素是否符合题意
        if (!checkDoublePair(pos)) return false;
        if (!checkDoublePair(neg)) return false;
        //原数组A中的元素都符合题意，返回true
        return true;
    }

    public static boolean checkDoublePair(int[] arr) {
        //从大到小遍历用于计数的数组
        //在这个数组中，数组下标i对应数组A中的一个元素绝对值
        //arr[i]是i这个值在数组A中出现的次数
        for (int i = arr.length - 1; i >= 0; i--) {
            //如果arr[i]>0说明i对应的计数还没有消除为0
            while (arr[i] > 0) {
                //i % 2 == 1说明i是奇数，在之前的操作中没有被消除
                //因为数组是从大到小遍历的，i是奇数，只能跟更大的数值配对
                //i是数组A中不符合题意的元素，返回false
                if (i % 2 == 1) return false;
                //如果i是偶数，该偶数可以与i/2配对
                //arr[i]--将i对应的计数减1
                arr[i]--;
                //查看与i配对的i/2的计数
                //arr[i / 2] == 0表示没有i/2与i组成数对
                //所以i不符合题意，返回false
                if (arr[i / 2] == 0) return false;
                //i/2可以和i配对，将其计数减1
                arr[i / 2]--;
            }
        }
        //数组所有元素遍历完成，都符合题意，返回true
        return true;
    }

```  

**复杂度分析**  

时间复杂度：O(n)，
忽略数组A的元素取值范围和数组长度限制，
对数组A一次遍历时间复杂度为n，
用于统计数组A中正数和负数的两个数组pos和neg，
两个数组的长度之和约等于数组A的长度n，
遍历pos和neg的方法虽然用了嵌套循环，
但是遍历过的元素不会重复操作，
所以对数组pos和neg遍历的时间复杂度仍然是O(n)，
综上所述，总的时间复杂度是O(n)  

空间复杂度：O(n)，
创建了两个数组，虽然根据题意有固定长度，
实际上可以假设这两个数组随着数组A的长度变化，
所以空间复杂度为O(n)

---  

**参考资料**  

* 英文官方题解：  
[https://leetcode.com/problems/array-of-doubled-pairs/solution/](https://leetcode.com/problems/array-of-doubled-pairs/solution/)  

* 网友高效数组方法：  
[https://leetcode-cn.com/submissions/api/detail/991/java/27](https://leetcode-cn.com/submissions/api/detail/991/java/27)  
