**692. 前K个高频单词**  
---
[https://leetcode-cn.com/problems/top-k-frequent-words/](https://leetcode-cn.com/problems/top-k-frequent-words/)  


```java  

    public List<String> topKFrequent(String[] words, int k) {
        //定义一个HashSet用来存储出现过的单词和计数
        Map<String, Integer> count = new HashMap();
        //遍历数组
        for (String word : words) {
            //count.getOrDefault(word, 0)获取set中已经存在的key为word的值
            //如果存在获取其计数，如果不存在给出默认值0
            //count.getOrDefault(word, 0) + 1 当前单词每出现一次计数加1
            count.put(word, count.getOrDefault(word, 0) + 1);
        }
        //提取set的所有key值，即words数组的全部元素，传入新建的List
        List<String> candidates = new ArrayList(count.keySet());

        //调用Collections自带的sort方法，该方法又调用List的sort方法，并最终使用了合并排序
        //第一个参数是实现了List接口的结合candidates
        //第二个参数是一个Comparator对象，在调用sort方法的同时，使用自定义的排序规则
        //(w1, w2) -> 使用了lambda表达式的写法
        /**
         Collections.sort(candidates, (w1, w2) -> count.get(w1).equals(count.get(w2)) ? w1.compareTo(w2) : count.get(w2) - count.get(w1));
         等价于
         Collections.sort(candidates, new Comparator<String>() {
             @Override
             public int compare(String w1, String w2) {
                 int result =  count.get(w1).equals(count.get(w2)) ? w1.compareTo(w2) : count.get(w2) - count.get(w1);
                 return result;
             }
         });
         *
         */
        // count.get(w1).equals(count.get(w2)) ? w1.compareTo(w2) : count.get(w2) - count.get(w1);
        // count.get(w1).equals(count.get(w2)) 首先比较两个单词的出现次数是否相等
        // w1.compareTo(w2) 如果w1和w2的计数相等，调用w1的compareTo方法，会给出两个单词的字母顺序比对结果
        // count.get(w2) - count.get(w1) 如果w1和w2的计数不相等，那么返回两个字符串出现次数的差值
        Collections.sort(candidates, (w1, w2) -> count.get(w1).equals(count.get(w2)) ?
                w1.compareTo(w2) : count.get(w2) - count.get(w1));
        //candidates.subList(0, k)截取最终结果的前k个并返回
        return candidates.subList(0, k);
    }

```  

**复杂度分析**  

时间复杂度: 
O(NlogN)，N是单词数组的长度，计算每个单词出现的频率耗费O(n)的时间复杂度，对单词排序耗费O(NlogN)的时间复杂度

空间复杂度: 
O(N), 使用了List存储n个单词

---


**参考资料**  

* 本题leetCode英文官方题解：  
[https://leetcode.com/articles/linked-list-cycle/](https://leetcode.com/articles/linked-list-cycle/)  


* Collections.sort()的用法和要点：  
[https://blog.csdn.net/wsll581/article/details/79953589](https://blog.csdn.net/wsll581/article/details/79953589)  
