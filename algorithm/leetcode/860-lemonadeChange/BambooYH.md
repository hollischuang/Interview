**柠檬水找零** 
---
[https://leetcode.com/problems/lemonade-change/](https://leetcode.com/problems/lemonade-change/)  
解决方案：  
方法一：**贪心**  
**思路**  
总共有5、10、20三种纸币，如果给的是5元，不用找零。如果给的是10元，肯定要找5元。关键给的是20元，如何找零。我们应该尽量找一张10元和一张5元，这种方法永远比找三张5元的要好。20元不会用于找零。  

**算法**  
我们用两个变量来分别记录5、10纸币的数量。然后根据上面所说规则进行找零。  
```
class Solution {
    public boolean lemonadeChange(int[] bills) {
        //res[0]记录5元纸币的数量，res[1]记录10元纸币的数量，20元的不用记录，因为不会用来找零
        int[] res = new int[2];
        for(int i = 0; i < bills.length; i++) {
            //如果付的是5元，不用找零
            if(bills[i] == 5) {
                res[0]++;
            //如果付的是10元，找5元
            }else if(bills[i] == 10) {
                if(res[0] == 0) {
                    return false;
                }
                res[0]--;
                res[1]++;
            //如果付的是20元，尽量找一张10元和一张3元，如果没有的话，在找三张5元。
            }else if(bills[i] == 20) {
                if(res[1] >= 1 && res[0] >= 1) {
                    res[1]--;
                    res[0]--;
                }else if(res[1] == 0 && res[0]>= 3) {
                    res[0] -= 3;
                }else {
                    return false;
                }
            }
        }
        return true;
    }
}
```
复杂度分析：  
假设数组长度为n  
时间复杂度：O(n)  
空间复杂度：O(1)