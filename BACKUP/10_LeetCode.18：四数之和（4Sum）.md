# [LeetCode 18：四数之和（4Sum）](https://github.com/lihe/MyLeetcode/issues/10)

[
 LeetCode 18：四数之和（4Sum）](https://leetcode.cn/problems/4sum/)


## **一、核心思路**





> **先排序，然后固定前两个数，把问题转化为一个有序数组上的 Two Sum，用双指针解决，并在每一层做去重。**



------





## **二、为什么一定要排序？**





排序后可以：



1. 使用 **双指针**（单调性）
2. **天然去重**（相同元素相邻）
3. 做 **剪枝优化**（提前 break / continue）





------





## **三、标准解法（O(n³)，面试推荐）**



```python
class Solution:
    def fourSum(self, nums, target):
        nums.sort()
        n = len(nums)
        res = []

        for i in range(n - 3):
            # 1️⃣ 第一层去重
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            # 剪枝：最小和都大于 target
            if nums[i] + nums[i+1] + nums[i+2] + nums[i+3] > target:
                break
            # 剪枝：最大和都小于 target
            if nums[i] + nums[n-1] + nums[n-2] + nums[n-3] < target:
                continue

            for j in range(i + 1, n - 2):
                # 2️⃣ 第二层去重
                if j > i + 1 and nums[j] == nums[j - 1]:
                    continue

                # 第二层剪枝
                if nums[i] + nums[j] + nums[j+1] + nums[j+2] > target:
                    break
                if nums[i] + nums[j] + nums[n-1] + nums[n-2] < target:
                    continue

                left, right = j + 1, n - 1

                while left < right:
                    total = nums[i] + nums[j] + nums[left] + nums[right]

                    if total == target:
                        res.append([nums[i], nums[j], nums[left], nums[right]])

                        # 3️⃣ 第三层去重（left）
                        while left < right and nums[left] == nums[left + 1]:
                            left += 1
                        # 4️⃣ 第四层去重（right）
                        while left < right and nums[right] == nums[right - 1]:
                            right -= 1

                        left += 1
                        right -= 1

                    elif total < target:
                        left += 1
                    else:
                        right -= 1

        return res
```



------





## **四、去重逻辑（这是这题的灵魂）**







### **一共** 

### **4 个位置**

###  **需要考虑去重：**



| **层级**  | **变量** | **去重方式**         |
| --------- | -------- | -------------------- |
| 第 1 个数 | i        | nums[i] == nums[i-1] |
| 第 2 个数 | j        | nums[j] == nums[j-1] |
| 第 3 个数 | left     | while 跳过           |
| 第 4 个数 | right    | while 跳过           |

👉 **任何一层不去重，都会产生重复四元组**



------





## **五、手动推演**



```
nums = [1,0,-1,0,-2,2]
target = 0
```

排序后：

```
[-2, -1, 0, 0, 1, 2]
```



### **i = -2, j = -1**





- left = 0, right = 2
- sum = -1 → left++
- sum = 0 → ✅ [-2, -1, 1, 2]







### **i = -2, j = 0**





- sum = 0 → ✅ [-2, 0, 0, 2]







### **i = -1, j = 0**





- sum = 0 → ✅ [-1, 0, 0, 1]





------





## **六、时间 & 空间复杂度**





- 时间复杂度：

  

  - 排序：O(n log n)
  - 两层循环 + 双指针：O(n³)

  

- 空间复杂度：

  

  - 排序栈空间：O(log n)
  - 输出不计入额外空间

  





------





## **七、常见错误（高频翻车点）**







### **❌ 错误 1：去重只做一层**





👉 结果中会出现重复四元组



------





### **❌ 错误 2：忘记排序**





👉 双指针和去重全部失效



------





### **❌ 错误 3：剪枝条件写反**



```
if min_sum > target: continue  # ❌ 应该 break
```



------





## **八、从 4Sum 到 kSum（重要升华）**







### **本质关系**



| **题目**  | **本质**              |
| --------- | --------------------- |
| Two Sum   | 哈希 / 双指针         |
| Three Sum | 固定 1 个 + Two Sum   |
| Four Sum  | 固定 2 个 + Two Sum   |
| kSum      | 固定 k-2 个 + Two Sum |



------





### **kSum 通用思想（一句话）**





> **排序 + 递归固定前 k-2 个数，底层用双指针做 Two Sum，并在每一层去重。**



------





## **九、面试标准回答模板（直接背）**





> 我先对数组排序，通过两层循环固定前两个数，再在剩余区间内使用双指针查找另外两个数，使四数之和等于 target。通过在每一层跳过重复元素来保证结果不重复，整体时间复杂度是 O(n³)。


