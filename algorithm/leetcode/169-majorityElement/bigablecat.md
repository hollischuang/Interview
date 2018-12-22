**169. 求众数**  
---
[https://leetcode-cn.com/problems/majority-element/](https://leetcode-cn.com/problems/majority-element/)  

* 官方题解2，hashMap  

```java  

    public int majorityElement(int[] nums) {
        //获取通过hashMap方法得到的数组中所有元素的计数
        Map<Integer, Integer> counts = countNums(nums);
        //临时变量用于存储hashMap中取出的众数
        Map.Entry<Integer, Integer> majorityEntry = null;
        //遍历hashMap中的每一个元素
        for (Map.Entry<Integer, Integer> entry : counts.entrySet()) {
            //如果众数临时变量majorityEntry为空，或者当前取出的数字计数比众数大
            if (majorityEntry == null || entry.getValue() > majorityEntry.getValue()) {
                //让众数临时变量等于当前元素
                majorityEntry = entry;
            }
        }
        //经过循环，得到计数最大的值，即所求的众数
        //majorityEntry是hashMap的元素，getKey()获得众数的数字
        return majorityEntry.getKey();
    }

    private Map<Integer, Integer> countNums(int[] nums) {
        //创建一个HashMap对象counts用于存储已经出现过的数字
        Map<Integer, Integer> counts = new HashMap<Integer, Integer>();
        //遍历int数组
        for (int num : nums) {
            //查看hashMap中是否已经存在当前数字
            if (!counts.containsKey(num)) {
                //如果不存在，使用当前数字做map的key，用计数1做value表示出现了1次
                counts.put(num, 1);
            } else {
                //如果已经存在，通过num这个key获得已经保存的value，即num的计数，在此基础上加1
                counts.put(num, counts.get(num) + 1);
            }
        }
        //返回hashMap
        return counts;
    }


```  

**复杂度分析**  

时间复杂度：O(n)，遍历数组时间复杂度O(n)，
遍历HashMap对象的所有元素时间复杂度也是O(n)，
最终时间复杂度为n+n，所以是O(n)

空间复杂度：O(n)，
众数在n个元素中最少出现的次数为 2/n+1，
那么非众数元素最多不会超过 n-(2/n+1) = n-2/n-1个，
众数本身也是一个元素，与其他非众数元素不同，
所以最坏情况下，n中总共有 (n-2/n-1)+1 = n-2/n个不同的元素
HashMap保存这些不同的元素需要占用n/2的空间，
所以空间复杂度是O(n/2)

---

* 官方题解5，递归和分治  

```java  

 public int majorityElement(int[] nums) {
        return majorityElementRec(nums, 0, nums.length - 1);
    }

    /**
     * 递归方法
     *
     * @param nums
     * @param lo
     * @param hi
     * @return
     */
    private int majorityElementRec(int[] nums, int lo, int hi) {
        //参数lo是数组首个元素的下标，参数hi是数组最后一个元素的下标，也是数组的长度
        if (lo == hi) {
            return nums[lo];
        }

        //获取数组的中位数元素下标
        // (hi - lo) / 2得到当前数组中间位置的元素距离首个元素的距离
        // (hi - lo) / 2 + lo得到数组中间元素的下标
        int mid = (hi - lo) / 2 + lo;
        //数组的左半部分从首个元素下标lo到中间元素下标mid
        int left = majorityElementRec(nums, lo, mid);
        //数组的右半部分从中间元素下标mid到最后一个元素下标hi
        int right = majorityElementRec(nums, mid + 1, hi);

        // 如果左右两边获得的众数相等，则该众数必定是整个数组的众数，直接返回
        if (left == right) {
            return left;
        }

        //统计左半边众数出现的总次数
        int leftCount = countInRange(nums, left, lo, hi);
        //统计右半边众数出现的总次数
        int rightCount = countInRange(nums, right, lo, hi);

        //返回较大的候选众数
        return leftCount > rightCount ? left : right;
    }


    /**
     * 计算候选众数在某个数组片段中出现的总次数
     *
     * @param nums
     * @param num
     * @param lo
     * @param hi
     * @return
     */
    private int countInRange(int[] nums, int num, int lo, int hi) {
        //定义一个计时器count
        int count = 0;
        //遍历从下标lo到下标hi的元素
        for (int i = lo; i <= hi; i++) {
            //如果获得的元素与当前传入的候选众数num相等，计数器加1
            if (nums[i] == num) {
                count++;
            }
        }
        //返回候选众数在当前数组片段中出现的总次数
        return count;
    }

```  

**复杂度分析**  

时间复杂度 : O(nlogn)，
每次递归，n就被2分一次，n/2/2...
所以总共调用递归方法的次数是logn次，
递归方法中有循环，最坏情况对每组进行了全员遍历，时间复杂度是O(n)，
所以总的时间复杂度是n*logn

空间复杂度：O(logn)，
因为进行了logn次的递归调用，
每次递归都占用O(1)的空间复杂度，
所以最终空间复杂度为O(logn)

---

**参考资料**  

* 英文官方题解：  
[https://leetcode.com/articles/majority-element/](https://leetcode.com/articles/majority-element/)  
