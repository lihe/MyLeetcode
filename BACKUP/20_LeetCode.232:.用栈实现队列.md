# [LeetCode 232: 用栈实现队列](https://github.com/lihe/MyLeetcode/issues/20)

[ LeetCode 232: 用队列实现栈](https://leetcode.cn/problems/implement-queue-using-stacks/)



# **一、核心思想**





> 让最新入队的元素，永远排到队列的最前面。



这样：

```
队头 = 栈顶
```



------





# **二、两个队列的角色分工**





我们用：



- q1：主队列（始终保存“栈顺序”）
- q2：辅助队列（临时倒腾用）





目标结构：

```
q1: [top, ..., bottom]
```

也就是说：



> **q1 的队头就是栈顶**



------





# **三、push(x) 的魔法**





假设当前栈是：

```
栈 = [1, 2]   (2 是栈顶)

q1 = [2, 1]
```

现在 push(3)



我们要让 3 成为“队头”。





### **步骤：**



```
1) 把 3 放进 q2
   q2 = [3]

2) 把 q1 所有元素依次倒进 q2
   q2 = [3, 2, 1]

3) 交换 q1 和 q2
   q1 = [3, 2, 1]
   q2 = []
```

🎯 现在队头 = 3 = 栈顶



------





# **四、pop / top / empty**





因为我们已经保证：

```
q1.front = 栈顶
```

所以：

| **操作** | **实现**       |
| -------- | -------------- |
| pop      | q1.pop_front() |
| top      | q1.peek()      |
| empty    | len(q1)==0     |



------





# **五、完整 Python 实现（标准解）**



```python
from collections import deque

class MyStack:

    def __init__(self):
        self.q1 = deque()
        self.q2 = deque()

    def push(self, x: int) -> None:
        # 新元素先进 q2
        self.q2.append(x)
        
        # 把 q1 全倒进 q2
        while self.q1:
            self.q2.append(self.q1.popleft())
        
        # 交换 q1 和 q2
        self.q1, self.q2 = self.q2, self.q1

    def pop(self) -> int:
        return self.q1.popleft()

    def top(self) -> int:
        return self.q1[0]

    def empty(self) -> bool:
        return not self.q1
```



------





# **六、复杂度分析（面试必考）**



| **操作** | **时间** |
| -------- | -------- |
| push     | O(n)     |
| pop      | O(1)     |
| top      | O(1)     |
| empty    | O(1)     |

你可以说：



> 通过在 push 时重排队列，把代价集中到 push 上。



------





# **七、进阶：只用一个队列**







### **思路：**





> push 后，把队列旋转，让新元素到队头



------





### **代码：**



```python 
class MyStack:
    def __init__(self):
        self.q = deque()

    def push(self, x: int) -> None:
        self.q.append(x)
        for _ in range(len(self.q) - 1):
            self.q.append(self.q.popleft())

    def pop(self) -> int:
        return self.q.popleft()

    def top(self) -> int:
        return self.q[0]

    def empty(self) -> bool:
        return not self.q
```



------





# **八、你应该记住的“对称思想”**



| **用什么** | **实现什么** |
| ---------- | ------------ |
| 两个栈     | 队列         |
| 两个队列   | 栈           |

两者核心都是：



> **用一次“整体倒置”，换来多次 O(1) 操作**


