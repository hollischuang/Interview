**模拟机器人行走**
---
https://leetcode.com/problems/walking-robot-simulation/

note: 这道题有个大坑：就是它要求机器人**最远走多远**，并不是你最终的位置。所以这道题才被标记为贪心，意思就是希望每走完一次，都要判以下最大值。一开始以为是我的程序的bug，废了我好长时间，真不值得。

### 思路一
看题目标题，意思就是模拟机器人行走。  
要额外的处理就是方向问题，机器人最开始面向北方，不妨我们使用4个数来表示东南西北吧。
- 0 ： 北方，对应坐标轴，y++
- 1 ： 东方：对应坐标轴，x++
- 2 ： 南方：对应坐标轴，y--
- 3 ： 西方：对应坐标轴，x--

机器人收到指令，无非是转向和前进：
- 当收到小于0的指令，便是转向。我们可以对于当前方向值进行更改，若向左转，则方向值减1，若当前为0，则要更新为3。若向右转，则方向加1，若当前为3，则要更新为0。原因：方向是个circle。
- 当收到大于0的指令，则是前进的指令。但是我们要一步一步走，因为中途可能碰到障碍。

另一个要解决的问题就是如何快速判断一个位置是否是障碍，O(1)的时间只能是**集合**了，又因为给定的是数组形式，所以我们自定义我们的hash函数为：`array[][0] + "-" + array[][1]`，如此便可以给每一个位置赋予唯一的key。

```java
    /**
     * 有点啰嗦的代码
     * @param commands
     * @param obstacles
     * @return
     */
    public int robotSim1(int[] commands, int[][] obstacles) {
        int x = 0, y = 0, direction = 0, maxDistance = 0;
        Set<String> obstaclesSet = new HashSet<>();
        for (int i = 0; i < obstacles.length; i++) {
            String str = obstacles[i][0] + "-" + obstacles[i][1];
            obstaclesSet.add(str);
        }
        for (int i = 0; i < commands.length; i++) {
            int value = commands[i];
            if (value > 0) {
                while (value-- > 0) {
                    switch (direction) {
                        case 0:
                            y++;break;
                        case 1:
                            x++;break;
                        case 2:
                            y--;break;
                        case 3:
                            x--;break;
                    }
                    String pos = x + "-" + y;
                    //若是障碍，要恢复如初
                    if (obstaclesSet.contains(pos)) {
                        switch (direction) {
                            case 0:
                                y--;break;
                            case 1:
                                x--;break;
                            case 2:
                                y++;break;
                            case 3:
                                x++;break;
                        }
                        //碰到障碍，即可立即结束此次指令
                        break;
                    }
                }
                maxDistance = Math.max(maxDistance, (int) (Math.pow(x, 2) + Math.pow(y, 2)));
            } else {
                if (value == -2) {
                    direction = direction - 1 == -1 ? 3 : direction - 1;
                } else {
                    direction = direction + 1 == 4 ? 0 : direction + 1;
                }
            }
        }
        return maxDistance;
    }
```
##### 简化
以上是我第一次写的，可能有点啰嗦，所以主要对恢复现场那进行简化。我们用一个二维数组预设前进的步伐，这样就可以用通用的代码应付各种不同的情景。
```java
    /**
     * 简洁的代码
     * @param commands
     * @param obstacles
     * @return
     */
    public int robotSim2(int[] commands, int[][] obstacles) {
        Set<String> set = new HashSet<>();
        for (int[] obs : obstacles) {
            set.add(obs[0] + "-" + obs[1]);
        }
        int[][] steps = new int[][]{{0,1}, {1, 0}, {0, -1}, {-1, 0}};
        int x = 0, y = 0, direction = 0, maxDistance = 0;
        for (int cmd : commands) {
            if (cmd == -2) {
                direction = direction - 1 == -1 ? 3 : direction - 1;
            } else if (cmd == -1) {
                direction =direction + 1 == 4 ? 0 : direction + 1;
            } else {
                while (cmd-- > 0 && !set.contains((x + steps[direction][0])
                        + "-" + (y + steps[direction][1]))) {
                    x += steps[direction][0];
                    y += steps[direction][1];
                }
                maxDistance = Math.max(maxDistance, (int) (Math.pow(x, 2) + Math.pow(y, 2)));
            }
        }
        return maxDi
```

参考：https://leetcode.com/problems/walking-robot-simulation/discuss/152322/Maximum!-This-is-crazy!