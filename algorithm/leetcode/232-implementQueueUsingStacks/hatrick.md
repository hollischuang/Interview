**225. 用队列实现栈**
---
[https://leetcode-cn.com/problems/implement-stack-using-queues/](https://leetcode.com/problems/backspace-string-compare/)

解决方案   
**思路**  
根据题意的描述,我们要使用队列实现栈,我们需要两个队列,来进行值得互换,一次来保证一个当前栈,进而实现栈的特性
**算法**  
从一个队列中拿出放到另外一个队列里面,然后队列里最后一个也就是我们的"栈顶元素" 
```
class MyStack {

    //使用两个队列来交换数据格式
    private Queue<Integer> q1 = new LinkedList<>();
    private Queue<Integer> q2 = new LinkedList<>();

    //保证当前的值只在一个队列里面
    public void push(int x) {
        if (!q1.isEmpty())
            q1.add(x);
        else
            q2.add(x);
    }

    public int pop() {
        //如果队列1位空
        if (q1.isEmpty()) {
            //则队列2有值,这里需要单独定义size 因为poll方法会使当前队列的长度动态变化
            int size = q2.size();
            //这里循环会留下q2队列最后添加的一项
            for (int i = 1; i < size; i++) {
                //将队列2从头开始取添加到队列1(保证当前只有一个队列里面有元素,弹栈之后值并不完整)
                q1.add(q2.poll());
            }
            //将对列2中头部元素弹栈并移除,也就是队列中的最后一个进入的
            return q2.poll();
        } else {
            //跟上面思路相同
            int size = q1.size();
            for (int i = 1; i < size; i++) {
                q2.add(q1.poll());
            }
            return q1.poll();
        }
    }

    public int top() {
        //定义临时变量
        int result;
        //如果q1为空
        if (q1.isEmpty()) {
            //这里的size方法需要单独提取出来
            int size = q2.size();
            //这里队列2会将最后进入的元素保留
            for (int i = 1; i < size; i++) {
                q1.add(q2.poll());
            }
            //拿到最后一个进入的元素赋值给临时变量
            result = q2.poll();
            //保证当前只有一个队列里面的元素是完整的
            q1.add(result);
        } else {
            int size = q1.size();
            for (int i = 1; i < size; i++) {
                q2.add(q1.poll());
            }
            result = q1.poll();
            q2.add(result);
        }
        return result;
    }

    public boolean empty() {
        return q1.isEmpty() && q2.isEmpty();
    }
}
```  
**复杂度分析**      
时间复杂度：O(N^2),需要取出当前队列里面的n-1个元素
空间复杂度：O(N^2),需要两个队列,取出放入组合
