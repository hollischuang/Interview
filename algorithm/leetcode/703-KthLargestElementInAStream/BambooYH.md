**703. 数据流中的第K大元素**
---
[https://leetcode.com/problems/kth-largest-element-in-a-stream/](https://leetcode.com/problems/kth-largest-element-in-a-stream/)  
**解决方案**  
方法一：**堆**  
**思路**  
这个题跟其他两题有点类似，一个是找给定数组中的第K大元素，一个是找数据流的中位数。我们分别讨论一下：  
数组中的第K大元素，这个我们一般用快排来找，因为首先数组元素的个数是固定的，而快排每趟都能确定一个数的最终位置，所以在给数组排序的过程中，就可以找到第K大的数。  
数据流中的第K大元素，因为是数据流，所以数据的个数是不固定的，这时候用快排就不合适了，因为最坏的情况下，第K大的元素一直在变，每次都需要重新排序。而且占用的空间也越来越大，这时候的空间复杂度和时间复杂度都是不能接受的。所以用最小堆是最合适的，我们只要保证最小堆的大小是K，那么堆顶的元素，肯定就是第K大的元素。同理可以用最大堆来找第K小元素  
数据流的中位数，这个题跟上一个还不一样，不但数据个数是不固定的，找的“第K大元素”也一直在变化。但因为每次找的都是中位数，所以我们可以用两个堆来实现，一个最大堆，一个最小堆。用两个堆来“平分”数据流  
**算法**  
创建一个最小堆，在将数据加入堆的过程中，如果待加入元素大于堆顶元素，则弹出堆顶元素，将待加入元素放入堆中，保证堆的大小是K，这样堆顶的元素就是我们要找的第K大的元素
```
 class KthLargest {
        //优先队列是用堆实现的
        final PriorityQueue<Integer> q;
        final int k;

        public KthLargest(int k, int[] a) {
            //初始化k和优先队列
            this.k = k;
            q = new PriorityQueue<>(k);
            for (int n : a)
                add(n);
        }

        public int add(int n) {
            //如果当前堆的元素个数小于K,则直接放入
            if (q.size() < k)
                q.offer(n);
            //如果待加入元素n大于堆顶元素，则弹出堆顶元素，将n放入堆中
            else if (q.peek() < n) {
                q.poll();
                q.offer(n);
            }
            //返回堆顶元素
            return q.peek();
        }
    }
```
**复杂度分析**  
空间复杂度：O(K)  
时间复杂度：O(n)，n为数据流的长度  

**参考资料**  
leetCode Discuss  
    [https://leetcode.com/problems/kth-largest-element-in-a-stream/discuss/149050/Java-Priority-Queue](https://leetcode.com/problems/kth-largest-element-in-a-stream/discuss/149050/Java-Priority-Queue)
