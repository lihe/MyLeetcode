# [LeetCode 215：数组中第 K 个最大的元素](https://github.com/lihe/MyLeetcode/issues/33)

这题是 **LeetCode 215：数组中第 K 个最大的元素**。

题目特别强调：



> **必须设计时间复杂度 O(n) 的算法**



所以 **不能直接排序 O(n log n)**，最经典的解法是：



> **Quickselect（快速选择算法）**



它和 **快速排序（QuickSort）** 很像，但只递归一侧，因此平均复杂度是 **O(n)**。



------





# **一、题目理解**





例如：

```
nums = [3,2,1,5,6,4]
k = 2
```

排序后：

```
[1,2,3,4,5,6]
```

第 2 大：

```
5
```

注意：

```
不是第2个不同元素
```



------





# **二、关键思路**





如果要找 **第 k 大**，等价于找：

```
第 n-k 小
```

例如：

```
n = 6
k = 2
```

第 2 大 = 第

```
6 - 2 = 4
```

小元素。



------





# **三、Quickselect 思想**





和快速排序一样：



1️⃣ 随机选一个 **pivot**

2️⃣ 把数组分成：

```
小于 pivot
pivot
大于 pivot
```

例如：

```
[3,2,1,5,6,4]
pivot = 4
```

分区后：

```
[3,2,1] 4 [5,6]
```

pivot 的最终位置：

```
index = 3
```

如果：

```
index == target
```

说明 pivot 就是答案。



否则：



- target 在左边 → 只递归左边
- target 在右边 → 只递归右边





------





# **四、代码实现（Quickselect）**



```python
import random

class Solution:
    def findKthLargest(self, nums, k):
        k = len(nums) - k
        
        def quickselect(l, r):
            pivot = nums[random.randint(l, r)]
            i, j = l, r
            
            while i <= j:
                while nums[i] < pivot:
                    i += 1
                while nums[j] > pivot:
                    j -= 1
                
                if i <= j:
                    nums[i], nums[j] = nums[j], nums[i]
                    i += 1
                    j -= 1
            
            if k <= j:
                return quickselect(l, j)
            if k >= i:
                return quickselect(i, r)
            
            return nums[k]
        
        return quickselect(0, len(nums) - 1)
```



------





# **五、执行过程示例**



```
nums = [3,2,1,5,6,4]
k = 2
```

转成：

```
target = 6 - 2 = 4
```

目标：

```
第4小
```

随机 pivot：

```
pivot = 4
```

分区：

```
[3,2,1] 4 [5,6]
```

pivot index = 3



target = 4 在右边



继续：

```
[5,6]
```

pivot = 6



最终找到：

```
5
```



------





# **六、复杂度**





平均情况：

```
O(n)
```

最坏情况：

```
O(n²)
```

但因为：

```
随机 pivot
```

几乎不会出现最坏情况。



------





# **七、为什么比排序快**





排序：

```
O(n log n)
```

Quickselect：

```
只递归一侧
```

复杂度：

```
O(n)
```



------





# **八、另一种常见解法（堆）**





用 **最小堆（heap）**：



维护大小为 k 的堆。

```python
import heapq

class Solution:
    def findKthLargest(self, nums, k):
        heap = nums[:k]
        heapq.heapify(heap)
        
        for num in nums[k:]:
            if num > heap[0]:
                heapq.heappop(heap)
                heapq.heappush(heap, num)
        
        return heap[0]
```

复杂度：

```
O(n log k)
```



------





# **九、面试推荐回答**





如果面试官问：



> 时间复杂度 O(n) 怎么做？



回答：



> 可以使用 Quickselect。它和快速排序类似，通过 partition 找到 pivot 的最终位置，如果 pivot 正好是目标位置就返回，否则只递归目标所在的一侧，因此平均复杂度是 O(n)。



------





# **十、记忆口诀**



```
第K大
↓
转成第 n-k 小
↓
Quickselect
↓
Partition
↓
只递归一边
```



------



如果你愿意，我可以再教你一个 **更稳定的大厂面试写法（Hoare Partition + Quickselect 模板）**，很多公司喜欢看这种写法。