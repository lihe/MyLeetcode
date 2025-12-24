# [LeetCode 206 反转链表](https://github.com/lihe/MyLeetcode/issues/1)




https://leetcode.cn/problems/reverse-linked-list/description/


## **一、题目回顾**





**问题**

给定单链表头节点 head，反转链表并返回反转后的头节点。



------





## **二、两种标准解法概览**





### **1️⃣ 迭代法（首插法 / 三指针）**



```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None
        cur = head
        while cur:
            nxt = cur.next
            cur.next = pre
            pre = cur
            cur = nxt
        return pre
```

**核心思想**

不断把当前节点 cur 插到新链表头部 pre 前。



------





### **2️⃣ 递归法（重点）**



```python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head is None or head.next is None:
            return head
        
        rev_head = self.reverseList(head.next)
        tail = head.next
        tail.next = head
        head.next = None
        return rev_head
```



------





## **三、递归反转链表的本质理解**







### **递归一句话定义**





> **我不反转整个链表，只负责把“当前节点”接到“已经反转好的后半段”后面。**



------





## **四、递归执行的完整过程（示例）**





链表：

```
1 → 2 → 3 → None
```

调用：

```
reverseList(1)
```



------





### **1️⃣ 递归下潜（只做调用，不反转）**



```
reverse(1)
 → reverse(2)
   → reverse(3)
```

在 head = 3 时触发 base case：

```
if head.next is None:
    return head
```

返回节点 3



------





### **2️⃣ 回溯阶段（真正发生反转）**





#### **回到head = 2**



```
tail = head.next      # 3
tail.next = head      # 3 → 2
head.next = None      # 2 → None
```

链表变为：

```
3 → 2 → None
```

返回 rev_head = 3



------





#### **回到head = 1**

```
tail = head.next      # 2
tail.next = head      # 2 → 1
head.next = None      # 1 → None
```

最终链表：

```
3 → 2 → 1 → None
```



------





## **五、为什么始终返回 rev_head？**



- rev_head **永远指向反转后链表的头节点**
- 它来自递归最深处（原链表尾部）
- 后续每一层只是在“接节点”，**不改变头指针**





❌ 错误理解：

“当前 head 是新的头” —— 这是错的



------





## **六、为什么必须写 head.next = None？** 





如果省略这一句：

```
tail.next = head
# 没有 head.next = None
```

会发生：

```
1 ↔ 2 ↔ 3
```

→ **形成环**



✅ 正确作用：



> 断开原有正向指针，避免产生环



------





## **七、递归反转链表的「通用模板」**





遇到链表递归题，直接套：



1️⃣ **Base Case**

空链表 / 单节点直接返回



2️⃣ **递归调用**

先处理 head.next



3️⃣ **局部操作**

修改当前节点与后继节点的指针关系



4️⃣ **返回结果**

始终返回“最深层”的头节点



------





## **八、迭代 vs 递归（建立直觉）**



| **方法** | **思维方式**           |
| -------- | ---------------------- |
| 迭代     | 我一步一步往前推       |
| 递归     | 我相信后面已经反转好了 |



------





## **九、🔥 高阶考点：带环链表会发生什么？**







### **输入：带环链表**



```
A → B → C → D → E
        ↑       ↓
        └───────┘
```



------





### **✅ 结论（面试官标准答案）**





**结论 1：不会死循环**



- 不会无限递归
- 不会栈溢出





**结论 2：最终结果是**



> **环内被反转，环外保持原样**



------





## **十、为什么不会死循环？**





关键不在「有没有环」，而在这里：

```
head.next = None
```



### **关键机制**





- 递归下潜时：**不会修改指针**
- 第一次回溯时：



```
head.next = None
```



- 👉 **直接剪断环**





一旦环被破坏：



- 后续递归不会再绕回
- 递归自然结束





📌 加分表述：



> 虽然输入是带环链表，但递归反转在回溯阶段会主动破坏环结构，因此不会死循环。



------





## **十一、为什么“环内反转，环外不动”？**





原因只有一句话：



> **只有参与回溯的节点，才会执行反转逻辑**





- 递归深入到环内后，才开始回溯
- 回溯只覆盖环内节点
- 环外节点的 next 从未被置为 None
- 因此顺序保持不变





------





### **最终结构示意**





原始：

```
A → B → C → D → E
        ↑       ↓
        └───────┘
```

结果：

```
A → B → C ← D ← E
        |
       None
```



------



## **十二、面试满分回答模板（可直接背）**





> 对于带环链表，这个递归反转函数不会死循环。

> 因为在回溯阶段会执行 head.next = None，第一次回溯就会破坏环结构，使递归能够正常终止。

> 最终结果是：环内节点被反转，环外节点保持原有顺序不变。


