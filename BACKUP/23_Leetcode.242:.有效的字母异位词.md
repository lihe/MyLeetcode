# [Leetcode 242: 有效的字母异位词](https://github.com/lihe/MyLeetcode/issues/23)


[242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)



## **一、核心思想（一句话版）**





> **两个字符串是字母异位词 ⇔ 每个字符出现的次数完全一样**



顺序不重要，**次数才重要**。



------





## **二、最优解（仅小写字母）——计数数组 O(n)**





因为题目明确说：



> 仅包含 a ~ z（26 个）





### **思路**





- 用长度为 26 的数组
- s 中字符出现：+1
- t 中字符出现：-1
- 最后只要有非 0 → ❌





------





### **Python 实现（面试最推荐）**



```python
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        cnt = [0] * 26

        for ch in s:
            cnt[ord(ch) - ord('a')] += 1

        for ch in t:
            cnt[ord(ch) - ord('a')] -= 1

        return all(c == 0 for c in cnt)
```



------





### **复杂度**





- 时间：**O(n)**
- 空间：**O(1)**（固定 26）





👉 这是**这道题的最优解**



------





## **三、为什么不能用排序？**



```
sorted(s) == sorted(t)
```



- 时间复杂度：O(n log n)
- 虽然能过，但**不是最优**





面试官更想看到你用 **计数思想**。



------





## **四、进阶：如果包含 Unicode 字符怎么办？**





题目进阶问得非常好，这是**真实工程问题**。





### **问题在哪里？**





- Unicode 字符集非常大
- 不能再用固定长度数组（比如 26）





------





## **五、Unicode 情况的正确解法 —— 哈希表（dict）**







### **思路**





- 用 dict 统计每个字符的出现次数
- 适用于任何字符集（中文、emoji、日文……）





------





### **Python 实现（通用版）**



```python 
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        if len(s) != len(t):
            return False

        freq = {}

        for ch in s:
            freq[ch] = freq.get(ch, 0) + 1

        for ch in t:
            if ch not in freq:
                return False
            freq[ch] -= 1
            if freq[ch] < 0:
                return False

        return True
```



------





### **复杂度**





- 时间：**O(n)**
- 空间：**O(n)**（取决于不同字符的数量）





------





## **六、Python 的“偷懒神器”**



```
from collections import Counter

Counter(s) == Counter(t)
```

✔ 可读性强

❌ 面试中**不推荐直接写**（会被追问原理）



------





## **七、面试一句话总结**





> 对于仅包含小写字母的情况，可以使用长度为 26 的计数数组统计字符频次；对于包含 Unicode 字符的情况，使用哈希表统计每个字符出现次数即可判断是否为字母异位词。



------


