**找数组中第K大的数**
---
[https://leetcode.com/problems/kth-largest-element-in-an-array/](https://leetcode.com/problems/kth-largest-element-in-an-array/)

近似题目  
[找数据流中第K大的数](https://github.com/hollischuang/Interview/tree/master/algorithm/leetcode/703-KthLargestElementInAStream)  

解决方案：  
方法一：**快排**  
**思路**  
找数组第K大的数,这个题通常会想到以下几种解法：  
1、先将数组排序，然后找到第K大的数。这种做法时间复杂度一般是O(nlogn)。  
2、用选择排序，找K次，就可以找到第K大的数。这种做法时间复杂度是O(n*K)  
3、快排。首先了解快排的思想，快排就是先确定一个比较元素cmp，然后遍历数组，比cmp大的都放到cmp右边，比cmp小的都放到cmp左边。遍历完一遍之后，cmp的位置就是排好序之后其所在的位置。也就是说每遍历一趟，都可以确定一个元素(cmp)的最终位置。所以我们可以利用快排的这个特点，来找到第K大的元素。  
**算法**  
首先假设数组的长度为n，则找第K大的数，就相当于找第(n-K+1)小的数。考虑到数组下标从0开始，我们将K = n - K。在快排的过程中，每遍历一趟，都会确定一个元素的最终位置，我们假设这个位置为index。我们将K和index进行比较，如果index < K，那说明，我们要找的元素在index的右面，继续在index的右边进行寻找，如果index > K,说明我们要找的元素在index的左边，继续在index的左边进行寻找，如果index == K,说明我们找到了第K大的元素。  
```
public class Solution {
    public int findKthLargest(int[] ele, int k) {
        int len = ele.length;
        return quickSort(ele,len-k,0,len-1);
    }
    public int quickSort(int[] ele, int k,int start, int end) {
        //如果start>end，说明并不存在第K大的数
        if(start > end)  
            return -1;
        int index = partition(ele,start,end);
        //如果index == K,说明找到了第K大的数
        if(index == k) {
            return ele[index];
        //如果index<K,说明要找的数，在index的右边
        } else if(index < k) {
            return quickSort(ele,k,index+1,end);
         //如果index>K,说明要找的数，在index的左边
        } else {
            return quickSort(ele,k,start,index-1);
        }
    }
    //返回比较元素cmp的位置
    public int partition(int[] ele, int left, int right) {
        //选第一个元素为比较元素
        int cmp = ele[right]; 
        int index = left - 1;
        for(int i = left; i < right; i++) {
            //如果当前元素小于cmp，则将该元素交换到cmp的前面
            if(ele[i] < cmp) {
                swap(ele,i,++index);
            }
        }
        //将cmp交换到最终的位置。
        swap(ele,right,++index);
        //返回cmp的位置
        return index;
    }
    //用位运算交换两个数的位置
    public void swap(int[] ele, int i, int j) {
        if(ele[i] == ele[j]) return;
        ele[i] ^= ele[j];
        ele[j] ^= ele[i];
        ele[i] ^= ele[j];
    }
}  

```
**复杂度分析**  
空间复杂度：O(1)  
时间复杂度：近似于O(n),一般情况下比快排的平均时间复杂度O(nlogn)要好一些。最坏情况下是O(n^2),这时候就是快排的最坏时间复杂度  

**拓展1**  
快排的过程中，比较元素的取法有很多种，可以选第一个，也可以选最后一个，也可以随机选一个.随机选一个的做法是，只需要将随机选的元素跟第一个或者最后一个交换。按照下面是各种选法的代码
```
//选最后一个元素为比较元素
    public static int partition1(int[] ele, int left, int right) {
        int pivot = ele[right];
        int index = left - 1;
        for(int i = left; i < right; i++) {
            if(ele[i] < pivot) {
                swap(ele,i,++index);
            }
        }
        swap(ele,right,++index);
        return index;
    }
    //选第一个元素为比较元素
    public static int partition2(int[] ele, int left, int right) {
         int pivot = ele[left];
         int index = left;
         for(int i = left + 1; i <= right; i++) {
             if(ele[i] <pivot) {
                 swap(ele,++index,i);
             }
         }
         swap(ele,index,left);
         return index;
    }
    //从两边往中间遍历
    public static int partition3(int[] ele, int left, int right) {
        int pivot = ele[left];
        while(left < right) {
            while(left < right && ele[right] > pivot) right--;
            if(left < right) {
                swap(ele,left,right);
                left++;
            }
            while(left < right && ele[left] < pivot) left++;
            if(left < right) {
                swap(ele,left,right);
                right--;
            }
        }
        return left;
    }

```
**拓展2**  
当数组中有大量重复元素的时候，用三项切分快排更合适，代码如下：
```
 public static void quickSort_3Way(int[] ele, int left, int right) {
        if(left >= right)
            return;
        //选取比较元素
        int pivot = ele[left];
        //遍历指针
        int leftScanPtr = left + 1;
        /*
        因为存在大量重复元素，所以跟pivot相等的元素可能有多个，所以遍历完一遍之后，pivot相等的值有多个，聚集在一起，lt用来记录其左端，rt用来记录其右端
        比如说比较元素是5，经过一趟遍历之后是324255578978，lt就是4，rt就是6
        */
        int lt = left;
        int rt = right;
        while(leftScanPtr <= rt) {
            if(ele[leftScanPtr] < pivot) {
                swap(ele,lt,leftScanPtr);
                leftScanPtr++;
                lt++;
            }else if(ele[leftScanPtr] > pivot) {
                swap(ele,rt,leftScanPtr);
                rt--;
            }else{
                leftScanPtr++;
            }
        }
        quickSort_3Way(ele,left,lt-1);
        quickSort_3Way(ele,rt+1,right);
    }
```

