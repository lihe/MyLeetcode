# [LeetCode 19：删除链表的倒数第 N 个节点](https://github.com/lihe/MyLeetcode/issues/14)


https://leetcode.cn/problems/remove-nth-node-from-end-of-list/



## **一、题目真正考什么？**





不是“删节点”，而是：



> **如何在一次遍历中，定位“倒数第 n 个节点的前一个节点”**



这件事用普通遍历是做不到的，

必须用 **快慢指针制造“距离差”**。



------





## **二、核心思路**





> 使用快慢指针，让快指针先走 n 步，然后快慢指针一起走，当快指针到达末尾时，慢指针正好指向要删除节点的前一个节点。



------





## **三、为什么一定要用 dummy（虚拟头节点）？**





因为要删除的节点 **可能是头节点**：



- [1], n = 1
- [1,2], n = 2





👉 dummy 可以 **统一处理所有情况**，不用特判。



------





## **四、标准解法（⭐一趟扫描，强烈推荐）**



```python 
class Solution:
    def removeNthFromEnd(self, head, n):
        dummy = ListNode(0)
        dummy.next = head

        fast = dummy
        slow = dummy

        # 1️⃣ fast 先走 n 步
        for _ in range(n):
            fast = fast.next

        # 2️⃣ fast 和 slow 一起走
        while fast.next:
            fast = fast.next
            slow = slow.next

        # 3️⃣ 删除倒数第 n 个节点
        slow.next = slow.next.next

        return dummy.next
```



------





## **五、指针推演**







### **示例 1**



```
head = [1,2,3,4,5], n = 2
```

加 dummy：

```
dummy → 1 → 2 → 3 → 4 → 5
↑
fast, slow
```



------





### **1️⃣ fast 先走 n = 2 步**



```
dummy → 1 → 2 → 3 → 4 → 5
          ↑
         fast
↑
slow
```



------





### **2️⃣ fast、slow 一起走**





直到 fast.next == None：

```
dummy → 1 → 2 → 3 → 4 → 5
                    ↑
                   fast
               ↑
              slow
```



------





### **3️⃣ 删除 slow.next（也就是 4）**



```
slow.next = slow.next.next
```

结果：

```
dummy → 1 → 2 → 3 → 5
```

返回 dummy.next。



------





## **六、为什么这是“一趟扫描”？**





虽然代码看起来有两段循环，但：



- fast 指针：只走一遍
- slow 指针：只走一遍





👉 **每个节点最多访问一次**

👉 时间复杂度仍然是 O(n)



------





## **七、时间 & 空间复杂度**





- 时间复杂度：O(n)
- 空间复杂度：O(1)





------





## **八、常见翻车点（非常重要）**







### **❌ 错误 1：不用 dummy**



```
fast = head
slow = head
```

👉 删除头节点时必崩



------





### **❌ 错误 2：fast 走 n-1 步**



```
for _ in range(n-1):  # ❌
```

👉 删除位置偏一位



------





### **❌ 错误 3：while fast 而不是 fast.next**



```
while fast:   # ❌
```

👉 slow 会多走一步



------




## **面试标准回答模板**





> 我使用快慢指针的方法，让快指针先走 n 步，然后快慢指针同步前进，当快指针到达链表末尾时，慢指针正好位于待删除节点的前一个节点，从而可以在一趟遍历中完成删除操作。



