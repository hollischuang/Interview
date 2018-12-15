**1. 两数之和**
---
[https://leetcode-cn.com/problems/two-sum/](https://leetcode-cn.com/problems/two-sum/)  

解决方案   
**思路**  
思路1: 根据题意我们其实可以直接使用双重for循环,然后拿到两个值相加就等于目标值的那两个下标返回即可,
只是这样的时间复杂度是O(N^2)
思路2: 我们可以转换思路,先将目标值与我们需要下标对应元素值保存起来,等到下一个我们需要的差值,就会拿到之前key,
既可以得到两个下标

```
private static int[] twoSum(int[] nums, int target) {
        //定义容器,存放首个下标,当有其差值出现的时候,便可以等到另一个下标
        Map<Integer, Integer> map = new HashMap<>();
        //用来存储最后返回结果
        int[] result = new int[2];
        for (int i = 0; i < nums.length; i++) {
            //判断之前的key是否已经保存到map中,如果已经存在,那么当前的值加上之前的值既为目标值
            if (map.containsKey(target - nums[i])) {
                result[0] = map.get(target - nums[i]);
                //这个时候i所对应的元素还没有放到map中,但是我们要找的值已经找到返回即可
                result[1] = i;
                return result;
            }
            map.put(nums[i], i);
        }
        return result;
    }

```  
**复杂度分析**      
时间复杂度：O(N) 循环所有数组元素,当然根据目标值,每次查找花费O(1)时间,所以最后复杂度为O(N)
空间复杂度：O(N) 使用map数据结构,存储的元素取决于传入的元素数量 


**参考资料**  
* 本题leetCode英文官方题解：  
[https://leetcode-cn.com/articles/two-sum/](https://leetcode-cn.com/articles/two-sum/)  
