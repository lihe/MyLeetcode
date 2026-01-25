# [Leetcode 202：快乐数](https://github.com/lihe/MyLeetcode/issues/24)

[202. 快乐数](https://leetcode.cn/problems/happy-number/)


对一个数 n 不断做这件事：



> 👉 把 n 的每一位数字拆出来

> 👉 求每一位数字的平方和

> 👉 得到一个新数，继续重复



结果只有两种可能：



- 最终变成 1 → **快乐数**
- 永远绕圈 → **不是快乐数**





------





> 如果某一步的结果**重复出现**，那后面一定会**进入死循环**



所以本质问题变成：



> **如何检测“重复状态”？**



------





# **方法一：哈希表**







## **思路**





- 用一个 set 记录 **已经出现过的数**

- 每次算平方和：

  

  - 如果等于 1 → 返回 True
  - 如果已经在 set 里 → 说明循环 → False

  





------





## **辅助函数：算“各位平方和”**



```python
def get_next(n):
    total = 0
    while n > 0:
        digit = n % 10
        total += digit * digit
        n //= 10
    return total
```



------





## **完整代码（哈希表版）**



```python
class Solution:
    def isHappy(self, n: int) -> bool:
        seen = set()
        
        while n != 1:
            if n in seen:
                return False
            seen.add(n)
            n = self.get_next(n)
        
        return True

    def get_next(self, n):
        total = 0
        while n > 0:
            digit = n % 10
            total += digit * digit
            n //= 10
        return total
```



------





## **复杂度**





- 时间：O(循环长度)（很小，几十步内）
- 空间：O(循环长度)





------





# **方法二：快慢指针**







## **核心思想（一句话）**





> 如果存在循环，

> **快慢指针一定会在环里相遇**



这和「判断链表是否有环」是**同一个模型**。



------





## **指针定义**





- slow：每次走一步
- fast：每次走两步





------





## **规则**





- 如果 fast 或 slow 变成 1 → 快乐数
- 如果 slow == fast 且不是 1 → 有环 → 非快乐数





------





## **完整代码（快慢指针版，O(1) 空间）**



```python
class Solution:
    def isHappy(self, n: int) -> bool:
        slow = n
        fast = self.get_next(n)

        while fast != 1 and slow != fast:
            slow = self.get_next(slow)
            fast = self.get_next(self.get_next(fast))

        return fast == 1

    def get_next(self, n):
        total = 0
        while n > 0:
            digit = n % 10
            total += digit * digit
            n //= 10
        return total
```



------





## **为什么一定能用快慢指针？**





因为：



- 数字平方和函数是 **确定性的**

- 每个数只会指向一个“下一个数”

- 整个过程本质是一个 **函数图**

- 一定会：

  

  - 到 1
  - 或进入一个环（如 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4）

  





------





## **三、面试如何选择写法？**



| **场景**         | **推荐**           |
| ---------------- | ------------------ |
| 一般面试         | 哈希表（清晰稳妥） |
| 要求 O(1) 空间   | 快慢指针           |
| 追问“还能优化吗” | 快慢指针           |



------





## **四、面试一句话总结**





> 快乐数问题本质是循环检测问题，可以使用哈希表记录中间结果判断是否出现重复，或者使用快慢指针在 O(1) 空间内检测是否存在循环。


