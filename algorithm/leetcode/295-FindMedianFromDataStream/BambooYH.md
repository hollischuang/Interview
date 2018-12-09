**找数据流的中位数**
---
[https://leetcode.com/problems/find-median-from-data-stream/](https://leetcode.com/problems/find-median-from-data-stream/)

近似题目  
[找数组中第K大的数](https://github.com/hollischuang/Interview/tree/master/algorithm/leetcode/215-KthLargestElementInAnArray)  
[找数据流中第K大的数](https://github.com/hollischuang/Interview/tree/master/algorithm/leetcode/703-KthLargestElementInAStream)  

解决方案：  
方法一：**堆**  
思路:  
首先，最容易想到的就是，每加进来一个数都重新排序，然后找中位数，但是这样的时间复杂度肯定是接受不了的。  
中位数可能是一个，也可能是两个，如果是两个的话，需要取平均值。这个题最重要的一点是，我们要有两个指针，可以随着数据流动态的记录两个中位数的位置或者值。如果总数是奇数，则两个指针指向同一个位置。所以这个题可以有多个解法。  
这里我们采用栈来实现，用一个最大栈，一个最小栈。在处理的过程中，我们要维持`|Size(MaxHeap) - Size(MinHeap)| <= 1`,这时候，两个栈的栈顶就相当于两个指针，他们始终指向中位数  
**算法**  
创建两个堆，最大堆和最小堆。向堆里加元素的算法是：  
1. 如果当前元素总数是奇数，则先将元素放入到最小堆，然后将最小堆堆顶的元素取出来，放入最大堆里面
2. 如果当前元素总数为偶数，则先将元素放入到最大堆里面，然后将最大堆堆顶的元素取出来，放入最小堆  
   
取中位数的算法是：  
1. 如果当前元素总数是奇数，则直接取最大堆堆顶的元素
2. 如果当前元素总数是偶数，则分别取最大堆和最小堆堆顶的元素，然后取平均值

代码:  
```
class MedianFinder {
    PriorityQueue<Integer> min;
    PriorityQueue<Integer> max;
    int count;//记录元素总数
    /** initialize your data structure here. */
    public MedianFinder() {
        //初始化堆和count
        count = 0;
        min = new PriorityQueue<Integer>();
        max = new PriorityQueue<Integer>(new Comparator<Integer>(){
            public int compare(Integer a, Integer b) {
                return b - a;
            }
        });
       
    }
    
    public void addNum(int num) {
        count++;
        //如果当前元素总数是奇数，则先将元素放入到最小堆，然后将最小堆堆顶的元素取出来，放入最大堆里面
        if((count & 1) == 1) {
            min.offer(num);
            max.offer(min.poll());
        // 如果当前元素总数为偶数，则先将元素放入到最大堆里面，然后将最大堆堆顶的元素取出来，放入最小堆 
        }else {
            max.offer(num);
            min.offer(max.poll());
        }
        
    }
    
    public double findMedian() {
        //如果当前元素总数是奇数，则直接取最大堆堆顶的元素
        if((count & 1) == 1) {
            return max.peek();
        //如果当前元素总数是偶数，则分别取最大堆和最小堆堆顶的元素，然后取平均值
        }else {
            return ((double)(max.peek()+min.peek()))/2;
        }
    }
}

```
复杂度分析:  
空间复杂度：O(n)，n为数据流的长度  
时间复杂度：O(logn)