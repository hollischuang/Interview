**232. 用栈实现队列**
---
[https://leetcode-cn.com/problems/implement-queue-using-stacks/](https://leetcode-cn.com/problems/implement-queue-using-stacks/)

解决方案   
**思路**  
根据题意的描述,我们要用栈实现队列先进先出的特性,使用两个栈,每次入栈之前,将栈中元素放入辅栈,在入栈之后再拉回来

```
class MyQueue {
    
    //设置一个flag标示位,表示每次第一个进栈的元素
    private int flag = -1;
    //主栈
    private Stack<Integer> s1 = new Stack<>();
    //辅助栈
    private Stack<Integer> s2 = new Stack<>();

    public MyQueue() {
    }

    public void push(int x) {
        //如果是第一次入栈 主栈和辅助栈都为空
        if (s1.empty()) {
            //第一次进来将flag置为进栈的值
            flag = x;
        }
        //如果第二次进来话,需要将前面进栈的值弹出,并放到辅助栈里面
        while (!s1.empty()) {
            s2.push(s1.pop());
        }
        //保证当前栈中push值得时候是干净的栈,模拟队列
        s1.push(x);
        //将辅助栈中的值弹出,放到主栈中,保证了主栈中先进先出的特性
        while (!s2.empty()) {
            s1.push(s2.pop());
        }
    }

    public int pop() {
        //直接弹栈 出来的就是最先进去的哪一个
        int num = s1.pop();
        if (!s1.empty()) {
            //然后将当前的s1栈顶的元素赋值给标志位
            flag = s1.peek();
        }
        return num;
    }

    public int peek() {
        //取标志位
        return flag;
    }

    public boolean empty() {
        return s1.empty();
    }
}

```  
**复杂度分析**      
时间复杂度：O(N^2)
空间复杂度：O(N^2)

**参考资料**  
   [https://blog.csdn.net/LaputaFallen/article/details/79998961?utm_source=blogxgwz5](https://blog.csdn.net/LaputaFallen/article/details/79998961?utm_source=blogxgwz5)