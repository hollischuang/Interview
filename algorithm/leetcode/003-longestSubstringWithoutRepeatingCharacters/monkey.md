**1. 003-LongestSubstringWithoutRepeatingCharacters**
---
[https://leetcode.com/problems/longest-substring-without-repeating-characters/](https://leetcode.com/problems/longest-substring-without-repeating-characters/)

- 解决思路
  > 题目需要求解最长无重复字符的子串长度。首先依次的遍历该字符串，用一个数据结构（set，map等）来保存已经遍历过的字符，当前字符没有出现过，则将其添加到我们定义的数据结构中，当前无重复子串长度加一。如果当前字符出现过，则需要将数据结构中与该字符相同字符之前的字符全部删除，并重新计数新的子串长度。此处需要用一个变量来保存之前无重复子串的最大长度，每次出现重复字符时都要跟之前最大的长度比较判断是否需要更新。

- code(scala version)
```
 def lengthOfLongestSubstringWithSet(s: String): Int ={
    //传入的string的长度
    var length = s.length
    //用set保存出现过的字符
    var elemSet: Set[Char] = Set()

    var begin, end = 0
    //用来保存当前所计算的最长长度
    var maxLength = 0
    while(begin < length && end < length){
      //如果set中不包含当前字符，则将当前字符添加入set，并向后移动给一个字符
      if(!elemSet.contains(s.charAt(end))){
        elemSet += s.charAt(end)
        end = end +1
        //更新maxLength，此处是重点：注意一定要用max函数比较 当前的不重复字符串长度 与 上一次出现重复字符时记录的最长不重复字符串长度
        maxLength = Math.max(maxLength , end - begin)
      } else{
        //如果set中包含当前字符，则利用循环将出现重复字符之前的字符全部从set中删除，保证set所留的都是需要重新计算长度的字符
        do {
          elemSet -= s.charAt(begin)
          begin = begin + 1
        } while(s.charAt(begin-1)!=s.charAt(end))
      }
    }

    maxLength
  }
```
- 时间空间复杂度分析
  > 时间复杂度：O(n) n为字符串长度，此处需要遍历两次字符串，所以时间复杂度为O(2n)=O(n)
  > 空间复杂度：O(min(m,n))  因为我们需要O(k)的空间来保存无重复字符的子串。k的最大长度不会超过原始字符串的长度n，也不会超过原始字符串中字符所在的字符表或者字母表集合的大小m。k取m和n的最小值。
  
- 参考资料
[https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/](https://leetcode.com/problems/longest-substring-without-repeating-characters/solution/)