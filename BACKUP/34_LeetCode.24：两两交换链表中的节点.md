# [LeetCode 24：两两交换链表中的节点](https://github.com/lihe/MyLeetcode/issues/34)

[24. 两两交换链表中的节点](https://leetcode.cn/problems/swap-nodes-in-pairs/)

关键要求：



> **不能修改节点的值，只能修改指针。**



也就是说：

```
1 → 2 → 3 → 4
```

要变成：

```
2 → 1 → 4 → 3
```

而不是把值改掉。



------





# **一、核心思路**





每次处理两个节点：

```
a → b → next
```

交换后变成：

```
b → a → next
```

然后继续处理后面的链表。



为了方便操作，我们一般加一个 **哨兵节点 dummy**。



------





# **二、图解过程**





原链表：

```
dummy → 1 → 2 → 3 → 4
          ↑
         cur
```

我们交换 1 和 2。



------





### **第一步：记录节点**



```
first  = cur.next      = 1
second = cur.next.next = 2
```



------





### **第二步：交换**





先连接 second：

```
cur.next = second
```

链表变成：

```
dummy → 2 → 3 → 4
```



------



再连接：

```
first.next = second.next
```

变成：

```
1 → 3
```



------



最后：

```
second.next = first
```

结果：

```
dummy → 2 → 1 → 3 → 4
```



------





### **第三步：移动 cur**



```
cur = first
```

继续处理：

```
3 → 4
```

最终：

```
2 → 1 → 4 → 3
```



------





# **三、标准代码（迭代）**



```python
class Solution:
    def swapPairs(self, head):
        dummy = ListNode(0)
        dummy.next = head
        cur = dummy

        while cur.next and cur.next.next:
            first = cur.next
            second = cur.next.next

            cur.next = second
            first.next = second.next
            second.next = first

            cur = first

        return dummy.next
```



------





# **四、代码逐行解释**







## **1 创建哨兵节点**



```
dummy = ListNode(0)
dummy.next = head
```

为什么要 dummy？



因为：

```
可能需要交换头节点
```



------





## **2 cur 指针**



```
cur = dummy
```

cur 表示：

```
当前需要交换的前一个节点
```



------





## **3 循环条件**



```
while cur.next and cur.next.next:
```

说明：

```
至少有两个节点
```

才能交换。



------





## **4 记录两个节点**



```
first = cur.next
second = cur.next.next
```



------





## **5 三步交换**



```
cur.next = second
first.next = second.next
second.next = first
```

顺序非常重要。



------





## **6 移动 cur**



```
cur = first
```

因为 first 现在是第二个节点。



------





# **五、完整执行示例**





输入：

```
1 → 2 → 3 → 4
```

步骤：





### **第一次**



```
swap(1,2)
```

变成：

```
2 → 1 → 3 → 4
```



------





### **第二次**



```
swap(3,4)
```

变成：

```
2 → 1 → 4 → 3
```



------





# **六、时间复杂度**





设链表长度 n



- 时间复杂度：



```
O(n)
```



- 空间复杂度：



```
O(1)
```



------





# **七、递归解法（更优雅）**





递归思路：

```
交换前两个
剩下的递归
```

代码：

```python
class Solution:
    def swapPairs(self, head):
        if not head or not head.next:
            return head

        first = head
        second = head.next

        first.next = self.swapPairs(second.next)
        second.next = first

        return second
```



------





# **八、递归执行过程**





例如：

```
1 → 2 → 3 → 4
```

第一层：

```
swap(1,2)
```

递归：

```
swap(3,4)
```

结果：

```
2 → 1 → 4 → 3
```



------





# **九、面试回答模板**





你可以这样回答：



> 这题可以通过修改指针实现相邻节点交换。每次记录当前节点的两个节点 first 和 second，然后调整三条指针完成交换，再移动指针继续处理后面的链表。时间复杂度 O(n)，空间复杂度 O(1)。



------





# **十、链表题套路总结**





这题属于链表常见的 **指针操作题**：



核心技巧：

| **技巧**    | **作用**              |
| ----------- | --------------------- |
| dummy节点   | 避免头节点特殊情况    |
| 记录3个指针 | first / second / next |
| 顺序修改    | 避免链表断开          |



------



如果你愿意，我可以帮你总结一份 **链表面试最常见的 10 道题模板**，基本覆盖大厂链表题。