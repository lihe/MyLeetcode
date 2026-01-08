# [LeetCode 151：反转字符串中的单词](https://github.com/lihe/MyLeetcode/issues/18)

[LeetCode 151：反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)







## 一、题目本质（先想清楚你到底在干嘛）





这题**不是简单反转字符串**，而是三件事同时发生：



1️⃣ **去掉多余空格**



- 前导空格 ❌
- 尾随空格 ❌
- 单词间多个空格 → 1 个空格





2️⃣ **以“单词”为单位反转顺序**



- 单词内部顺序 **不变**
- 单词整体顺序 **反转**





3️⃣ 返回一个 **规范化的新字符串**



------





## **二、Python 最优解（面试 & 实战首选）**







### **✅ 推荐写法（一句话版）**



```python
class Solution:
    def reverseWords(self, s: str) -> str:
        return " ".join(s.split()[::-1])
```



### **为什么这一行就够？**





我们拆解一下：

```
s.split()
```



- 自动：

  

  - 去掉前导 / 尾随空格
  - 合并多个空格

  

- 得到：**单词数组**



```
[::-1]
```



- 反转单词顺序



```
" ".join(...)
```



- 用 **单个空格** 拼接





------





### **示例推演**



```
s = "  a good   example  "

s.split()
# ['a', 'good', 'example']

[::-1]
# ['example', 'good', 'a']

join
# "example good a"
```

✔ 完全符合题意



------





## **三、复杂度分析**





- 时间复杂度：O(n)
- 空间复杂度：O(n)（split 会创建新数组）





⚠️ **Python 中这是最合理的解法**



------





## **四、进阶：O(1) 空间的“原地解法”思路**





> ⚠️ 仅适用于 **字符串是可变类型**（如 char[]）





### **原地解法三步走（一定要记住）**







#### **1️⃣ 去除多余空格（压缩）**



```
"  a  good   example  "
→ "a good example"
```



#### **2️⃣ 整体反转字符串**



```
"a good example"
→ "elpmaxe doog a"
```



#### **3️⃣ 逐个单词反转**



```
"elpmaxe doog a"
→ "example good a"
```



------





### **为什么这样可行？**





- 整体反转 → **单词顺序反了**
- 单词再反转 → **单词内部顺序恢复**





👉 **反转的经典组合技**



------





## **五、为什么 Python 不强求原地？**





你可以这样回答面试官：



> Python 的字符串是不可变类型，无法真正原地修改；在 Python 中通常通过 split 和 join 返回新字符串即可。如果是字符数组场景，可以使用三步反转法在 O(1) 额外空间内完成。



------





## **六、常见翻车点**







### **❌ 错误 1：直接反转字符串**



```
s[::-1]  # ❌
```

会得到：

```
"eulb si yks eht"
```

👉 **单词内部顺序也被反了**



------





### **❌ 错误 2：只 split，不处理空格**



```
s.split(" ")  # ❌
```

会产生空字符串：

```
['', '', 'hello', '', 'world', '', '']
```



------





### **❌ 错误 3：join 用错分隔符**



```
"".join(...)  # ❌
```



------





## **七、面试标准回答模板**





> 我先将字符串按空格分割成单词数组，split 会自动去除多余空格；然后反转单词数组，最后使用单个空格拼接，得到反转后的字符串。


## 变形

反转单词顺序，但“中间的空格不压缩、不合并”，原来有几个空格就保留几个空格



### **变形题的精确定义（常见问法）**





- **单词**：连续的非空格字符
- **空格块**：连续的空格（可能是 1 个、2 个、很多个）
- 要做的是：**把单词整体顺序反过来，但所有空格块原样保留在原来的位置/形态**





举例：



- "a good  example" → "example good  a"
- " hello  world " → " world  hello "（如果也要求保留前后空格，那就这样；下面的方案会连前后一起保留，最稳）





------





## **面试可背解法：分成“单词块 / 空格块”，只反转单词块**







### **思路**





1. 扫描字符串，按顺序提取 token：要么是一段空格 "  "，要么是一段单词 "hello".
2. 把所有单词收集起来并反转。
3. 再按原 token 序列重建：遇到“空格 token”就原样放回；遇到“单词 token”就从反转后的单词表里依次取。







### **Python 实现**



``` python
class Solution:
    def reverseWords_keep_spaces(self, s: str) -> str:
        tokens = []   # 按原顺序保存：('space', '   ') 或 ('word', 'hello')
        words = []

        n, i = len(s), 0
        while i < n:
            j = i
            if s[i] == ' ':
                while j < n and s[j] == ' ':
                    j += 1
                tokens.append(('space', s[i:j]))
            else:
                while j < n and s[j] != ' ':
                    j += 1
                w = s[i:j]
                tokens.append(('word', w))
                words.append(w)
            i = j

        words.reverse()

        # 重建：空格原样保留；单词用反转后的顺序替换
        res = []
        wi = 0
        for typ, val in tokens:
            if typ == 'space':
                res.append(val)
            else:
                res.append(words[wi])
                wi += 1

        return ''.join(res)
```



------





## **复杂度**





- 时间：O(n)
- 额外空间：O(n)（Python 字符串不可变，想做到严格 O(1) 很难；面试一般接受）





------





## **面试官追问你怎么答**





> 为什么不能用 split()？





- split() 会把多个空格合并（或者产生空串），**会破坏“保留中间所有空格”的要求**。



