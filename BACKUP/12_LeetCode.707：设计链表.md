# [LeetCode 707：设计链表](https://github.com/lihe/MyLeetcode/issues/12)

https://leetcode.cn/problems/design-linked-list/description/






# **一、设计选择**





👉 **选择：单链表 + 虚拟头节点（dummy） + 记录长度**



为什么？

| **设计** | **好处**                 |
| -------- | ------------------------ |
| 单链表   | 结构简单、好写           |
| dummy    | 所有操作不用特判 index=0 |
| size     | 快速判断 index 合法性    |

这套设计是 **最稳、最不容易出 bug 的**。



------





# **二、链表节点定义**



```python 
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next
```



------





# **三、MyLinkedList 完整实现**



```python
class MyLinkedList:

    def __init__(self):
        self.dummy = ListNode(0)
        self.size = 0

    def get(self, index):
        if index < 0 or index >= self.size:
            return -1

        cur = self.dummy.next
        for _ in range(index):
            cur = cur.next
        return cur.val

    def addAtHead(self, val):
        self.addAtIndex(0, val)

    def addAtTail(self, val):
        self.addAtIndex(self.size, val)

    def addAtIndex(self, index, val):
        if index < 0 or index > self.size:
            return

        prev = self.dummy
        for _ in range(index):
            prev = prev.next

        node = ListNode(val)
        node.next = prev.next
        prev.next = node
        self.size += 1

    def deleteAtIndex(self, index):
        if index < 0 or index >= self.size:
            return

        prev = self.dummy
        for _ in range(index):
            prev = prev.next

        prev.next = prev.next.next
        self.size -= 1
```



------





# **四、逐个方法讲清楚“为什么这样写”**







## **1️⃣** 

## **get(index)**



```
if index < 0 or index >= self.size:
    return -1
```



- 链表 **不能随机访问**
- 必须从头走 index 步





👉 时间复杂度：O(index)



------





## **2️⃣** 

## **addAtHead(val)**



```
self.addAtIndex(0, val)
```

🔥 **关键点**：

**所有插入统一走 addAtIndex**



------





## **3️⃣** 

## **addAtTail(val)**



```
self.addAtIndex(self.size, val)
```

当 index == size：



👉 插入到尾部（题目明确允许）



------





## **4️⃣** 

## **addAtIndex(index, val)**







### **核心逻辑**



```
prev = self.dummy
for _ in range(index):
    prev = prev.next
```



- 找到 **index 前一个节点**
- 插入操作永远是：



```
prev → new → prev.next
```



### **插入代码**



```
node.next = prev.next
prev.next = node
```



------





## **5️⃣** 

## **deleteAtIndex(index)**







### **删除的本质**





删除 index 节点

👉 **找到 index-1 的节点**

```
prev.next = prev.next.next
```

dummy 的存在让：



- 删除头节点
- 删除中间节点





**写法完全一样**



------





# **五、为什么 dummy 是“王炸设计”？**





如果不用 dummy：



- index == 0 要特判
- 删除头节点要改 head
- 插入头节点要写两套逻辑





用了 dummy：



> **所有操作都当“中间节点”处理**



这是链表题的**工程级思维**。



------





# **六、复杂度分析**



| **操作**      | **时间复杂度** |
| ------------- | -------------- |
| get           | O(n)           |
| addAtHead     | O(1)           |
| addAtTail     | O(n)           |
| addAtIndex    | O(n)           |
| deleteAtIndex | O(n)           |

空间复杂度：O(1)（不算节点本身）



------





# **七、常见翻车点**







### **❌ 忘记更新 size**



```
self.size += 1
self.size -= 1
```



------





### **❌ index 合法范围写错**





- 插入：index > size ❌
- 删除：index >= size ❌





------





### **❌ 删除后还移动指针**



```
prev = prev.next  # ❌ 删除时不能动
```



------







# **一句话终极总结**





> 通过虚拟头节点和长度记录，可以统一处理所有边界情况，使链表的增删查操作逻辑简洁且稳定。



------




你选一个，我继续。