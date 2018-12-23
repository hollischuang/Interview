**柠檬水找零**
---
https://leetcode.com/problems/lemonade-change/
### 思路一
这道题属于典型的贪心算法。贪心算法的思路就是优先给当前状态分配最优解。子问题最优，从而促进父问题也最优。

回归问题，有这么个几种情况：
1. 顾客有5元，那再好不过了
2. 顾客有10元，那要看看你当前有没有5元，如果没有，gg了
3. 顾客有20元，这时要优先给他10元。因为5元很宝贵啊，5元可以适用于10，20元的情况，而10元只能适用于20元情况。给顾客10元，从而节省更多的5元为后面10元的顾客找零，这就是贪心的体现，为当前分配最优解。

基于以上的讨论，我们只需两个计数器统计当前剩余的5元，10元即可。
```java
    /**
     * 贪心做法
     * @param bills
     * @return
     */
    public boolean lemonadeChange1(int[] bills) {
        int five = 0;
        int ten = 0;
        for (int i = 0; i < bills.length; i++) {
            int value = bills[i];
            if (value == 5) { //5元，计数
                five++;
            } else if (value == 10) { //10元，5减一，10加1
                if (five == 0) {
                    return false;
                }
                five--;
                ten++;
            } else if (value == 20) {
                //有10元，先给10元
                if (ten != 0) {
                    ten--;
                    value = 10;
                }
                //尝试给5元
                while (value > 5  && five > 0) {
                    value -= 5;
                    five--;
                }
                //若最终无法减为5元，说明找不开，gg
                if (value > 5) {
                    return false;
                }
            }
        }
        return true;
    }
```

上面的代码太啰嗦了，简化以下，瞬间清爽。
```java
    public boolean lemonadeChange2(int[] bills) {
        int five = 0;
        int ten = 0;
        for (int i = 0; i < bills.length; i++) {
            int value = bills[i];
            if (value == 5) {
                five++;
            } else if (value == 10) {
                five--;
                ten++;
            } else if (ten > 0) {
                ten--;
                five--;
            } else {
                five -= 3;
            }
            if (five < 0) {
                return false;
            }
        }
        return true;
    }

```
参考：https://leetcode.com/problems/lemonade-change/discuss/143719/C%2B%2BJavaPython-Straight-Forward