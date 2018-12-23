**242. 有效的字母异位词**  
---
[https://leetcode-cn.com/problems/valid-anagram/](https://leetcode-cn.com/problems/valid-anagram/)  

* 官方题解方法1：排序后对比是否相等  

```java  

    public boolean isAnagram(String s, String t) {
        if (s == null || t == null) return false;
        //如果长度不等，直接返回false
        if (s.length() != t.length()) return false;

        //将两个字符串转换为字符数组char[]
        char[] sChar = s.toCharArray();
        char[] tChar = t.toCharArray();

        //使用Arrays的sort方法分别为两个字符数组排序
        //Arrays.sort使用的DualPivotQuickSort在经典快排基础上改进，时间复杂度稳定为O(nlogn)
        Arrays.sort(sChar);
        Arrays.sort(tChar);

        //比较排序后的两个字符数组是否相等
        return Arrays.equals(sChar, tChar);
    }

```  

**复杂度分析**  

时间复杂度：O(nlogn)，假设n是s的长度
排序的时间复杂度O(nlogn)，对比两个字符串的时间复杂度O(n)
总的复杂度是 nlogn+n，舍弃n，所以最终复杂度是O(nlogn)

空间复杂度：O(1)，如果使用堆排序，需要O(1)的辅助空间；
本题的Java解法toCharArray有复制原字符串的行为，所以使用了O(n)的辅助空间

---

<br>  

* 官方题解方法2：用计数器统计每个字符出现的次数  

```java  

	if (s == null || t == null) return false;
	//如果两个字符串长度不同，直接返回false
	if (s.length() != t.length()) {
	    return false;
	}
	// 假设单词里的字符都在a~z的范围内，创建一个长度为26的int数组作为计数器
	// 数组中每个元素的默认值都是0，相当于为26个英文字母逐个建立了计数器
	int[] counter = new int[26];
	//遍历字符串s
	for (int i = 0; i < s.length(); i++) {
	    //s.charAt(i)获取当前字符
	    //s.charAt(i) - 'a' 得到当前字符与a的差，数值在0~25之间
	    //counter[s.charAt(i) - 'a']从counter中获取当前字符所在位置的计数
	    //counter[s.charAt(i) - 'a']++ 将当前字符在counter中的计数值+1
	    counter[s.charAt(i) - 'a']++;
	    //同理，将t中当前字符在counter的计数值-1
	    counter[t.charAt(i) - 'a']--;
	}
	//当所有字符遍历完成后，如果每个字符都出现了相同的次数，counter中所有元素都将归零
	//遍历counter查看计数器数组是否已经归零
	for (int count : counter) {
	    //出现非0的情况，说明有不同的字符
	    if (count != 0) {
	        return false;
	    }
	}
	return true;
        
```  

**复杂度分析**  

时间复杂度: O(n). 遍历s的长度，时间复杂度为n

空间复杂度: O(1)，使用的counter数组容量是常数所以空间复杂度为 O(1)

---


**参考资料**  

* 本题leetCode英文官方题解：  
[https://leetcode.com/articles/valid-anagram/](https://leetcode.com/articles/valid-anagram/)  


* Collections.sort()的用法和要点：  
[https://blog.csdn.net/wsll581/article/details/79953589](https://blog.csdn.net/wsll581/article/details/79953589)  
