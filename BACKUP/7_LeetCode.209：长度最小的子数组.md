# [LeetCode 209：长度最小的子数组](https://github.com/lihe/MyLeetcode/issues/7)

https://leetcode.cn/problems/minimum-size-subarray-sum/description/

------





## **一、这题为什么能用 O(n)？**





题目里有一个**隐藏但决定性的条件**：



> **nums 中全是正整数**



这意味着：



- 窗口右扩 → 和 **只会变大**
- 窗口左缩 → 和 **只会变小**





👉 **窗口具有单调性**

👉 **可以用滑动窗口，而不是暴力枚举**



------





## **二、O(n) 解法：滑动窗口**







### **核心思想**





> 用两个指针维护一个窗口，右指针负责扩展直到满足条件，左指针负责收缩以缩短长度。



------





## **三、标准代码**



```python
class Solution:
    def minSubArrayLen(self, target, nums):
        left = 0
        curr_sum = 0
        res = float('inf')

        for right in range(len(nums)):
            curr_sum += nums[right]

            while curr_sum >= target:
                res = min(res, right - left + 1)
                curr_sum -= nums[left]
                left += 1

        return 0 if res == float('inf') else res
```



------





## **四、手动走一遍**







### **示例 1**



```
target = 7
nums = [2,3,1,2,4,3]
```

初始化：

```
left=0, curr_sum=0, res=∞
```



### **过程关键片段：**





1️⃣ right=3 → curr_sum = 8

满足条件，开始收缩：



- res = 4
- 去掉 nums[0]=2 → sum=6（不满足）





2️⃣ right=4 → curr_sum = 10

收缩：



- res = 4
- 去掉 nums[1]=3 → sum=7
- res = 3
- 去掉 nums[2]=1 → sum=6（停止）





3️⃣ right=5 → curr_sum = 9

收缩：



- res = 3
- 去掉 nums[3]=2 → sum=7
- res = 2
- 去掉 nums[4]=4 → sum=3





最终：

```
res = 2
```



------





## **五、为什么时间复杂度是 O(n)？**





虽然看起来有两层循环：

```
for right in ...
    while curr_sum >= target
```

但注意：



- right 只走一遍
- left 也只走一遍





👉 **每个元素最多被加一次、减一次**



所以是：

```
O(n)
```



------





## **六、常见错误**







### **❌ 错误 1：用 if 而不是 while**



```
if curr_sum >= target:   # ❌
```

👉 你会错过更短的子数组



------





### **❌ 错误 2：没意识到“正整数”是前提**





如果 nums 里有负数：



- 滑动窗口 ❌ 不成立
- 要换方法（前缀和 + 二分 / 哈希）





------





### **❌ 错误 3：忘记返回 0 的情况**





------





## **七、进阶：O(n log n) 解法**







### **思路**





- 用 **前缀和**
- 对每个起点 i
- 二分找最小的 j，使：





$prefix[j] - prefix[i] \ge target$



------





### **代码**



```python
import bisect

class Solution:
    def minSubArrayLen(self, target, nums):
        n = len(nums)
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i + 1] = prefix[i] + nums[i]

        res = float('inf')
        for i in range(n):
            need = target + prefix[i]
            j = bisect.bisect_left(prefix, need)
            if j <= n:
                res = min(res, j - i)

        return 0 if res == float('inf') else res
```



------





### **复杂度**





- 构建前缀和：O(n)
- 每次二分：O(log n)
- 总体：O(n log n)





------





## **八、O(n) vs O(n log n) 怎么选？**





> 因为数组元素全为正数，滑动窗口具有单调性，所以可以用 O(n) 的双指针解法。

> 如果不满足单调性，或者需要支持更复杂查询，可以用前缀和加二分。



------





## **九、这道题在“滑动窗口体系”中的地位**





这是 **最标准的滑动窗口模型**：



- 固定条件：sum >= target
- 优化目标：**窗口长度最小**





你后面会遇到大量同构题：



- 最长 / 最短子数组
- 满足条件的子串
- 至多 / 至少 K 个元素





👉 **模板一模一样**






看到题目出现：



- 连续子数组
- 全是正数
- 求最短 / 最长


**第一反应：滑动窗口**



