# [LeetCode 24：两两交换链表中的节点](https://github.com/lihe/MyLeetcode/issues/13)


https://leetcode.cn/problems/swap-nodes-in-pairs/




## **一、题目核心约束（先抓重点）**





- ❌ 不能交换 val
- ✅ 只能交换 **节点本身（next 指针）**
- 每次交换 **相邻两个节点**
- 节点个数可能是 0 / 1 / 奇数 / 偶数





👉 这直接排除了“偷懒交换值”的做法。



------





## **二、最稳解法：**

## **dummy + 迭代（强烈推荐）**







### **一句话思路**





> 使用一个虚拟头节点，把链表每两个节点当成一组，通过指针重连完成交换，然后移动到下一组。



------





## **三、标准代码（面试级，零特判）**



``` python 
class Solution:
    def swapPairs(self, head):
        dummy = ListNode(0)
        dummy.next = head
        prev = dummy

        while prev.next and prev.next.next:
            first = prev.next
            second = first.next

            # 交换两个节点
            prev.next = second
            first.next = second.next
            second.next = first

            # 移动到下一组
            prev = first

        return dummy.next
```



------





## **四、指针交换理解**







### **初始状态（示例 1）**



```
dummy → 1 → 2 → 3 → 4
          ↑
         prev
```



------





### **第一次交换（1 和 2）**



```
prev.next = 2
1.next = 3
2.next = 1
```

结果：

```
dummy → 2 → 1 → 3 → 4
               ↑
              prev
```

> ⚠️ 注意：prev 移动到 **交换后这一组的第二个节点（first）**



------





### **第二次交换（3 和 4）**



```
dummy → 2 → 1 → 4 → 3
```

结束。



------





## **五、为什么一定要用 dummy？**





如果不用 dummy：



- 交换第一组时
- 你必须单独处理头节点
- 代码会分成两套逻辑





👉 **dummy 的作用：统一所有情况**



> 删除 / 交换头节点 = 删除 / 交换中间节点



这是链表题的“工程级技巧”。



------





## **六、while 条件为什么是这样？**



```
while prev.next and prev.next.next:
```

含义：



- 至少还剩 **两个节点**

- 自动处理：

  

  - 空链表 []
  - 单节点 [1]

  





------





## **七、时间 & 空间复杂度**





- 时间复杂度：O(n)
- 空间复杂度：O(1)（迭代写法）





------





## **八、递归解法（理解用，可选）**







### **思路**





- 交换前两个
- 剩下的递归处理







### **代码**



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

📌 面试推荐 **迭代版**，因为无额外栈空间。



------





## **九、常见翻车点（非常重要）**







### **❌ 错误 1：交换顺序写错**





指针顺序一错就断链 / 成环。



------





### **❌ 错误 2：prev 移动错**



```
prev = second  # ❌ 会死循环
```



------





### **❌ 错误 3：交换 val（违规）**



```
a.val, b.val = b.val, a.val  # ❌
```



------





## **十、面试标准回答模板**





> 通过引入一个虚拟头节点来统一处理头节点的交换问题，在遍历过程中每次将相邻的两个节点重新连接，并将指针移动到下一组，整个过程只修改 next 指针，不修改节点值，时间复杂度是 O(n)。



------





看到题目出现：



- 链表
- 成对 / 成组操作
- 不能改值





👉 **dummy + 成组指针重连**



------



