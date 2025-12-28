# [LeetCode 203：移除链表元素](https://github.com/lihe/MyLeetcode/issues/11)


https://leetcode.cn/problems/remove-linked-list-elements/



## **一、这道题的“真正难点”是什么？**





不是删中间节点，而是：



> **当要删除的节点可能是头节点时，如何统一处理？**



例如：



- [6,1,2] 删除 6
- [6,6,6] 删除 6
- [] 空链表





👉 如果你单独处理头节点，代码会变得又臭又长。



------





## **二、最稳妥的解法：**

## **虚拟头节点（dummy node）**







### **一句话思路**





> 在原链表前加一个虚拟头节点，用它统一处理所有删除逻辑。



------





## **三、标准解法（强烈推荐）**



```python 
class Solution:
    def removeElements(self, head, val):
        dummy = ListNode(0)
        dummy.next = head

        cur = dummy
        while cur.next:
            if cur.next.val == val:
                cur.next = cur.next.next
            else:
                cur = cur.next

        return dummy.next
```



------





## **四、为什么要用 dummy**







### **如果不用 dummy，会发生什么？**





你必须写很多特判：

```
while head and head.val == val:
    head = head.next
```

然后再处理后面的节点……



👉 **逻辑分裂，容易漏情况**



------





### **dummy 的好处**



```
dummy → head → ...
```



- dummy 永远不删
- 所有删除操作都变成：



```
cur.next = cur.next.next
```

👉 **删除头节点和删除中间节点，写法完全一致**



------





## **五、手动推演一遍**







### **示例 1**



```
head = [1,2,6,3,4,5,6], val = 6
```

加 dummy：

```
0 → 1 → 2 → 6 → 3 → 4 → 5 → 6
↑
cur
```

过程关键点：



- cur.next = 6 → 删除
- cur 不动（因为可能连续是 6）
- 最终链表：



```
0 → 1 → 2 → 3 → 4 → 5
```

返回：

```
dummy.next → [1,2,3,4,5]
```



------





### **示例 3（全删）**



```
head = [7,7,7,7], val = 7
```

结果：

```
dummy → None
```

返回 None，正确。



------





## **六、为什么是 while cur.next，而不是 while cur？** 





因为我们判断的是：

```
cur.next.val == val
```

如果 cur.next 是 None，就不能访问 .val 了。



👉 **这是链表题里非常经典的写法**



------





## **七、时间 & 空间复杂度**





- 时间复杂度：O(n)
- 空间复杂度：O(1)（dummy 是常数级）





------





## **八、常见错误（高频翻车）**







### **❌ 错误 1：不处理头节点**



```
cur = head
while cur.next:
    ...
```

👉 头节点如果等于 val，会漏删



------





### **❌ 错误 2：删除后还移动 cur**



```
cur.next = cur.next.next
cur = cur.next   # ❌
```

👉 会跳过连续的 val



------





### **❌ 错误 3：空链表没处理**





dummy 写法自动解决这个问题。






## **九、面试标准回答模板**





> 我通过引入一个虚拟头节点来统一处理删除逻辑，从而避免对头节点进行特殊判断。遍历过程中，如果当前节点的下一个节点值等于目标值，就跳过该节点，否则继续向后移动，最终返回虚拟头节点的下一个节点。



------




看到题目出现：



- 删除链表节点
- 可能删除头节点
- 删除条件是某个值





👉 **第一反应：dummy 虚拟头节点**

