**15. 三数之和**  
---
[https://leetcode-cn.com/problems/3sum/](https://leetcode-cn.com/problems/3sum/)  


解决方案   
**思路**  
首先考虑数组遍历时去重:
方法一：先将数组排好序,在遍历的时候与上一个进行比较,相同则直接进入下一个
方法二：用容器Set——简单,但是同样需要排序,增加算法复杂度并且此题三个数操作不方便,
　　　　降低复杂度一般的途径就是利用已有的而未用到的条件将多余的步骤跳过或者删去,由于三个数是具有一个特点的:和为某个定值，这个条件只是用来判断了而并没有使用
　　　　并且,由上个去重得知，后面使用的数组是已经排序好的。此时仔细想想应该就能想到,从两端使用两个指针相向移动,两端指针所指数之和如果小于目标值,只需要移动左边的指针,否则只需要移动右边的指针!!
        例如[1,1,1,4,5,7,8,8,9]中 定目标值为15，从两边开始，1+9为10，小于15，移动右边指针左移变成1+8只会更少，所以移动左边变成4+9以此类推

```
  public static List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        Arrays.sort(nums);

        for (int i = 0; i < nums.length - 2; i++) {
            int left = i + 1;
            int right = nums.length - 1;
            if (i > 0 && nums[i] == nums[i - 1])
                continue; // 去掉重复的起点
            while (left < right) {
                int sum = nums[left] + nums[right] + nums[i];
                if (sum == 0) {
                    result.add(Arrays.asList(nums[i], nums[left], nums[right]));
                    while (left < right && nums[left] == nums[left + 1])
                        left++; // 去掉重复的左点
                    while (left < right && nums[right] == nums[right - 1])
                        right--; // 去掉重复的右点
                    right--; // 进入下一组左右点判断
                    left++;
                } else if (sum > 0) {
                    right--; // sum>0 ,说明和过大了，需要变小，所以移动右边指针
                } else {
                    left++; // 同理，需要变大，移动左指针
                }
            }
        }
        return result;
  }

```  
**复杂度分析**      
时间复杂度：O(N2) 使用排序+去重+双指针移动定位 
空间复杂度：O(N)


**参考资料**    
[https://www.cnblogs.com/Xieyang-blog/p/8242900.html](https://www.cnblogs.com/Xieyang-blog/p/8242900.html)  
