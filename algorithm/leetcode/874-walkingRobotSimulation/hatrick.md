**874.模拟机器人行走**
---
[https://leetcode.com/problems/walking-robot-simulation/](https://leetcode.com/problems/walking-robot-simulation/)

解决方案   
**思路**  
机器人在(0,0)点开始行走，如果(0,0)点有障碍怎么办,这种情况是不管它,开始下一步行走,一旦机器人离开(0,0)点,这个点的障碍物才生效,
后面如果回到此点则不能跨过此障碍.也就是说我们需要先走一步,再去判断这一步是否有效,有效则更新坐标,否则原地不动,继续下一次动作.
至于右转和左转实际就是改变机器人的朝向,起初机器人向北,右转则朝向东,左转则朝向西.不同的朝向,意味着向前一步改变的坐标形式不一样,
如果朝北,则x不动,y递增;如果朝南,则x不动,y递减;如果朝西,则x递减,y不动;如果朝东,则x递增,y不动;也就是在每次实施除了左转右转的动作
(调整朝向)之外的移动操作时,需要知道机器人的朝向.知道了朝向就往前走呗,遇到了前方障碍物则不要往前,结束此次动作,开始下一次动作.

在方向的表示层面,用坐标来表示方向不仅可以很好的确定方向之间的关系,而且还能确定此方向上的增量.考虑[0,1],[1,0],[0,-1],[-1,0]分别代表北,东,南,西.
可以看到每个坐标的左边就是它的左转方向,右边的就是它的右边方向,也就是说,给一个方向i(方向向量的索引),则左转的方向是i-1,右转的方向是i+1.等等,那两头呢,
开始的位置不能减1,末端的位置不能加1.把首和尾连接器起来就好了,因此要进行取余操作,长度为4.当然了(0-1)%4没意义,因此改写成((0-1)+4)%4,即左转为（i+3）%4,
这样对中间位置和起始位置都适用.方向确定好了,那坐标增量呢,注意到某一方向上行进一步的增量恰好就是该方向的坐标.以北为例,y方向增量是1,x方向增量是0,也就是(0,1).

```
class Solution {
    public int robotSim(int[] commands, int[][] obstacles) {
        int max = 0;
        int[][] dx = {{0, 1}, {1, 0}, {0, -1}, {-1, 0}};
        int k = 0;
        Map<String, Boolean> map = new HashMap<>();
        for (int i = 0; i < obstacles.length; i++) {
            map.put(obstacles[i][0] + "," + obstacles[i][1], true);
        }
        int p = 0, q = 0;
        for (int command : commands) {
            if (command == -1) {
                k = (k + 1) % 4;
            } else if (command == -2) {
                k = (k + 4 - 1) % 4;
            } else {
                int cur[] = dx[k];
                for (int i = 0; i < command; i++) {
                    if (map.containsKey((p + cur[0]) + "," + (q + cur[1]))) {
                        break;
                    }
                    p += cur[0];
                    q += cur[1];
                }
                max = Math.max(max, p * p + q * q);
            }
        }
        return max;
    }
}

```  
**复杂度分析**      
时间复杂度：O(N+M)  N和M分别是两个数组的长度
空间复杂度：O(N)    使用map所占用的空间

**参考资料**  
   [https://blog.csdn.net/qq_37976559/article/details/82228460](https://blog.csdn.net/qq_37976559/article/details/82228460)
   [https://blog.csdn.net/Jeff_Winger/article/details/81544085](https://blog.csdn.net/Jeff_Winger/article/details/81544085)