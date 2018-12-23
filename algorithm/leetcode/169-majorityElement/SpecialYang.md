**169. 求众数**  
---
[https://leetcode.com/problems/majority-element/](https://leetcode.com/problems/majority-element/)  

### 思路一

因为题目明确定义了众数的概念，即出现次数大于[n / 2]的数，那么**排序后的数组最中间的那个数必为众数**。  
你可能会有疑问，有序数组如果没有众数，那么中间那个数就不是众数。But，题目强调了给定的数组必有众数。
```java
    /**
     * 排序法
     * @param nums
     * @return
     */
    public int majorityElement1(int[] nums) {
        Arrays.sort(nums);
        return nums[nums.length / 2];
    }   
```
#### 复杂度
- 时间复杂度：此思路主要消耗在排序算法上，所以时间复杂度取决于你用的什么排序算法。
- 空间复杂度：同上

### 思路二
同样，众数的概念为出现次数大于n / 2次。那么就会有以下算法，俗称阵地法：
1. 一个值pivot用来表示当前出现次数超过0的数字，另一个值count表示它出现的次数
2. 从头开始遍历数组
3. 若保存的数字pivot的出现次数为0，那么就把pivot更新为当前的值，并且次数 + 1
4. 若保存的数字的次数不为0，且与当前的值相等，那么次数 + 1
5. 若保存的数字的次数不为0，且与当前的值不相等，那么次数 - 1
6. 直到遍历完整个数组，最终pivot的值必为出现次数大于[n / 2]的数
```java
    /**
     * 阵地法
     * @param nums
     * @return
     */
    public int majorityElement2(int[] nums) {
        int pivot = 0;
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (i == 0 || count == 0) {
                pivot = nums[i];
                count++;
            } else if (pivot != nums[i]) {
                count--;
            } else {
                count++;
            }
        }
        return pivot;
    }
```
#### 复杂度
- 时间复杂度：需要遍历一遍数组，所以O(n)
- 空间复杂度：需要哨兵和计数器，所以O(1)

### 思路三
众数的概念为出现次数大于n / 2次，那么数组第[n / 2]大的数必为众数。

基于快排的partition方法油然而生，随机选择一个哨兵，然后把所有不大于它的数全部移动到它的左边，把所有大于的数全部移动到它的右边。判断哨兵所在的索引与n / 2 的大小关系，若大于，说明中间值在左部分，按同样的逻辑递归处理左部分；若小于，说明中间值在右部分，按同样的逻辑递归处理右部分。直到相等。
```java
    /**
     * 基于快排的划分
     * @param nums
     * @param low
     * @param high
     * @param target
     */
    private void partition(int[] nums, int low, int high, int target) {
        if (low < high) {
            int end = low + new Random().nextInt(high - low + 1);
            swap(nums, end, high);
            int index = low;
            for (int i = low; i < high; i++) {
                if (nums[i] < nums[high]) {
                    swap(nums, i, index);
                    index++;
                }
            }
            swap(nums, index, high);
            if (index < target) {
                partition(nums, index + 1, high, target);
            } else if (index > target) {
                partition(nums, low, index - 1, target);
            }
        }
    }

    /**
     * 交换函数
     * @param nums
     * @param i
     * @param j
     */
    private void swap(int[] nums, int i, int j) {
        if (i != j) {
            int temp = nums[i];
            nums[i] = nums[j];
            nums[j] = temp;
        }
    }
```
#### 复杂度
- 时间复杂度：快排的partition的时间复杂度为O(n)，详细证明可参考算法导论
- 空间复杂度：尾递归，不需要额外空间，O(1)

### 参考
1. [剑指Offer-30-数组中出现次数超过一半的数字](https://blog.csdn.net/dawn_after_dark/article/details/81152544)