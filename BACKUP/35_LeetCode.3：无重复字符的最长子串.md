# [LeetCode 3：无重复字符的最长子串](https://github.com/lihe/MyLeetcode/issues/35)

[3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)

核心一句话：



> **用一个窗口维护“当前没有重复字符的子串”，右指针不断扩展，出现重复时左指针收缩。**



------





# **一、为什么这是滑动窗口题**





因为题目要求的是：



- **子串**（连续）
- 满足某种条件（无重复字符）
- 求最长长度





这正是滑动窗口的典型特征。



------





# **二、核心思路**





我们维护一个窗口：

```
s[left ... right]
```

要求这个窗口内：

```
所有字符都不重复
```

做法：



1. right 不断向右扩展，把新字符加入窗口
2. 如果发现重复，就移动 left 缩小窗口，直到重新满足“无重复”
3. 每次更新最大长度





------





# **三、最推荐写法：用集合维护窗口**



```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        char_set = set()
        left = 0
        ans = 0

        for right in range(len(s)):
            while s[right] in char_set:
                char_set.remove(s[left])
                left += 1

            char_set.add(s[right])
            ans = max(ans, right - left + 1)

        return ans
```



------





# **四、代码逐行解释**







## **1. 初始化**



```
char_set = set()
left = 0
ans = 0
```



- char_set：当前窗口里的字符
- left：窗口左边界
- ans：记录最长长度





------





## **2. 右指针遍历**



```
for right in range(len(s)):
```

让右边界不断往右移动。



------





## **3. 如果重复，就缩窗口**



```
while s[right] in char_set:
    char_set.remove(s[left])
    left += 1
```

如果新字符已经在窗口里，说明重复了。

那就不断移走左边字符，直到重复消失。



------





## **4. 加入当前字符**



```
char_set.add(s[right])
```



------





## **5. 更新答案**



```
ans = max(ans, right - left + 1)
```

当前窗口长度就是：

```
right - left + 1
```



------





# **五、手动走示例**







## **示例 1**



```
s = "abcabcbb"
```

过程大概是：



- "a" → 长度 1
- "ab" → 长度 2
- "abc" → 长度 3
- 加入下一个 "a" 时重复，左边开始缩
- 窗口变成 "bca"，长度还是 3
- 最终最长长度是 3





答案：

```
3
```



------





## **示例 2**



```
s = "bbbbb"
```

窗口始终最多只能保留一个 "b"，所以答案：

```
1
```



------





## **示例 3**



```
s = "pwwkew"
```

最长无重复子串是：

```
"wke"
```

长度：

```
3
```

注意 "pwke" 不是连续的，所以不是子串。



------





# **六、复杂度分析**





- 时间复杂度：O(n)
- 空间复杂度：O(k)，其中 k 是字符集大小，最坏 O(n)





为什么时间是 O(n)？



因为：



- right 每个字符访问一次
- left 也只会一直往右走，不回头





所以整体是线性的。



------





# **七、更常见的进阶写法：用字典记录字符最后位置**





这个写法更高效，也更常见于面试高分答案。

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        last = {}
        left = 0
        ans = 0

        for right, ch in enumerate(s):
            if ch in last and last[ch] >= left:
                left = last[ch] + 1

            last[ch] = right
            ans = max(ans, right - left + 1)

        return ans
```



------





# **八、这个字典写法为什么更好**





集合写法在遇到重复时：

```
一个一个删
```

字典写法直接：

```
把 left 跳到重复字符上次出现位置的后一位
```

所以逻辑更干净。



------





# **九、面试回答模板**





你可以这样回答：



> 这题可以用滑动窗口解决。用左右指针维护一个不含重复字符的窗口，右指针不断扩展；如果遇到重复字符，就移动左指针缩小窗口，直到窗口重新满足无重复条件。遍历过程中不断更新窗口最大长度，时间复杂度是 O(n)。



------





# **十、一句话记忆**





> **最长无重复子串 = 滑动窗口 + 哈希表/集合**



------



如果你愿意，我可以继续给你讲这题的：



- **集合版和字典版有什么区别**
- **为什么** **left = max(left, last[ch] + 1)** **更稳**
- **滑动窗口题的一页通杀模板**