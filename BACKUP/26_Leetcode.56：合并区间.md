# [Leetcode 56：合并区间](https://github.com/lihe/MyLeetcode/issues/26)

[56. 合并区间](https://leetcode.cn/problems/merge-intervals/)



# **一、核心思路（一句话）**





> **先按区间起点排序，再从左到右合并**



------





# **二、为什么一定要排序？**





假设输入：

```
[[4,7],[1,4]]
```

如果不排序，你无法判断哪个在前。



排序后：

```
[[1,4],[4,7]]
```

区间关系变得“线性可判断”。



------





# **三、排序后发生了什么？**





排序后，区间一定满足：

```
intervals[i][0] >= intervals[i-1][0]
```

我们只需要判断：

```
当前区间 start <= 上一个区间 end
```

如果满足 → 重叠

否则 → 不重叠



------





# **四、合并规则（核心）**





设当前合并区间为：

```
[cur_start, cur_end]
```

新来区间：

```
[start, end]
```



------





## **情况 1：重叠**



```
start <= cur_end
```

合并：

```
cur_end = max(cur_end, end)
```



------





## **情况 2：不重叠**



```
start > cur_end
```



- 把当前区间加入结果
- 更新当前区间





------





# **五、完整代码（面试标准写法）**



```python
class Solution:
    def merge(self, intervals: list[list[int]]) -> list[list[int]]:
        if not intervals:
            return []

        # 1️⃣ 先按起点排序
        intervals.sort(key=lambda x: x[0])

        res = []
        cur_start, cur_end = intervals[0]

        # 2️⃣ 线性扫描
        for start, end in intervals[1:]:
            if start <= cur_end:
                # 重叠 → 扩展右边界
                cur_end = max(cur_end, end)
            else:
                # 不重叠 → 保存当前区间
                res.append([cur_start, cur_end])
                cur_start, cur_end = start, end

        # 3️⃣ 最后一个别忘记加入
        res.append([cur_start, cur_end])

        return res
```



------





# **六、用示例走一遍**





输入：

```
[[1,3],[2,6],[8,10],[15,18]]
```

排序后：

```
[[1,3],[2,6],[8,10],[15,18]]
```

过程：

```
[1,3] + [2,6] → [1,6]
[1,6] + [8,10] → 不重叠
[8,10] + [15,18] → 不重叠
```

输出：

```
[[1,6],[8,10],[15,18]]
```



------





# **七、时间复杂度**





- 排序：O(n log n)
- 扫描：O(n)





👉 总复杂度：**O(n log n)**



这是这题的最优解。



------





# **八、区间题统一模型（非常重要）**





这题是下面所有题的基础：

| **题目**   | **核心**    |
| ---------- | ----------- |
| 合并区间   | 排序 + 合并 |
| 插入区间   | 分类 + 合并 |
| 区间交集   | 双指针      |
| 会议室问题 | 排序比较    |
| 覆盖区间   | 贪心        |

👉 **区间问题 = 排序 + 贪心**



------





# **九、面试一句话总结（可以背）**





> 先按区间起点排序，然后线性遍历区间，如果当前区间与上一个区间重叠，则合并右边界，否则加入结果列表，时间复杂度 O(n log n)。



------





# **十、常见面试追问**







### **Q：为什么可以贪心？**





因为：



> 排序后，局部最优（尽量扩展当前区间）一定是全局最优。



------





### **Q：如果输入是流式数据怎么办？**





不能排序 → 要用平衡树 / 优先队列



（这个是进阶）







可以的：这题的关键仍然是 **“先排序，再线性合并”**，只不过把 intervals.sort() 换成你自己手撕的排序。



下面给你一份**面试最稳**的版本：用 **归并排序（Merge Sort）** 手写排序（稳定、最坏 O(n log n)），然后做合并。



------





## **手撕归并排序 + 合并区间（不使用 sort）**



```python
from typing import List

class Solution:
    def merge(self, intervals: List[List[int]]) -> List[List[int]]:
        if not intervals:
            return []

        # 1) 手撕排序：按 start 升序；start 相同按 end 升序
        intervals = self.merge_sort(intervals)

        # 2) 合并
        res = []
        cur_s, cur_e = intervals[0]
        for s, e in intervals[1:]:
            if s <= cur_e:                 # 重叠（题目把 [1,4] 和 [4,5] 也算重叠）
                cur_e = max(cur_e, e)
            else:
                res.append([cur_s, cur_e])
                cur_s, cur_e = s, e
        res.append([cur_s, cur_e])
        return res

    def merge_sort(self, arr: List[List[int]]) -> List[List[int]]:
        n = len(arr)
        if n <= 1:
            return arr

        mid = n // 2
        left = self.merge_sort(arr[:mid])
        right = self.merge_sort(arr[mid:])

        return self.merge_two(left, right)

    def merge_two(self, a: List[List[int]], b: List[List[int]]) -> List[List[int]]:
        i = j = 0
        out = []

        # 比较规则：先比 start，再比 end
        while i < len(a) and j < len(b):
            if a[i][0] < b[j][0] or (a[i][0] == b[j][0] and a[i][1] <= b[j][1]):
                out.append(a[i])
                i += 1
            else:
                out.append(b[j])
                j += 1

        # 收尾
        if i < len(a):
            out.extend(a[i:])
        if j < len(b):
            out.extend(b[j:])

        return out
```



------





## **复杂度**





- 排序：O(n log n)（归并排序最坏也成立）
- 合并：O(n)
- 总：O(n log n)，空间 O(n)（归并的临时数组）



