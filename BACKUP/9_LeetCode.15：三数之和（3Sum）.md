# [LeetCode 15：三数之和（3Sum）](https://github.com/lihe/MyLeetcode/issues/9)



https://leetcode.cn/problems/3sum/


## **一、这道题的关键难点是什么？**





不是“能不能找到三个数”，而是：



> **如何在不重复的前提下，找全所有和为 0 的三元组**



核心难点只有两个：



1. **去重**
2. **把暴力 O(n³) 降到 O(n²)**





------





## **二、核心思路**







### **一句话版本**





> **先排序，然后固定一个数，用双指针在右侧区间内找另外两个数。**



------





## **三、为什么要排序？**





排序后可以：



1. 使用双指针（利用单调性）
2. 轻松去重（相同元素会相邻）





------





## **四、标准解法（O(n²)，强烈推荐）**



```python 
class Solution:
    def threeSum(self, nums):
        nums.sort()
        res = []
        n = len(nums)

        for i in range(n):
            # 1️⃣ 剪枝：最小值已经 > 0，不可能再凑出 0
            if nums[i] > 0:
                break

            # 2️⃣ 去重：跳过重复的 nums[i]
            if i > 0 and nums[i] == nums[i - 1]:
                continue

            left, right = i + 1, n - 1

            while left < right:
                total = nums[i] + nums[left] + nums[right]

                if total == 0:
                    res.append([nums[i], nums[left], nums[right]])

                    # 3️⃣ 去重：跳过重复的 left
                    while left < right and nums[left] == nums[left + 1]:
                        left += 1
                    # 4️⃣ 去重：跳过重复的 right
                    while left < right and nums[right] == nums[right - 1]:
                        right -= 1

                    left += 1
                    right -= 1

                elif total < 0:
                    left += 1
                else:
                    right -= 1

        return res
```



------





## **五、手动走一遍**







### **示例**



```
nums = [-1,0,1,2,-1,-4]
```

排序后：

```
[-4, -1, -1, 0, 1, 2]
```



### **i = 1（nums[i] = -1）**





- left = 2（-1）
- right = 5（2）
- sum = 0 → 记录 [-1, -1, 2]





继续：



- left = 3（0）
- right = 4（1）
- sum = 0 → 记录 [-1, 0, 1]





完成。



------





## **六、为什么这样能避免重复？**







### **去重一共** 

### **三层**







#### **1️⃣ 固定 i 时去重**



```
if i > 0 and nums[i] == nums[i - 1]:
    continue
```

避免同一个数作为第一个元素重复出现。



------





#### **2️⃣ left 指针去重**



```
while left < right and nums[left] == nums[left + 1]:
    left += 1
```



------





#### **3️⃣ right 指针去重**



```
while left < right and nums[right] == nums[right - 1]:
    right -= 1
```



------





## **七、时间 & 空间复杂度**





- 时间复杂度：

  

  - 排序：O(n log n)
  - 双指针：O(n²)
  - 总体：**O(n²)**

  

- 空间复杂度：

  

  - 排序：O(log n)（Python 内部）
  - 结果集不算额外空间

  





------





## **八、常见错误**







### **❌ 错误 1：忘记排序**





👉 双指针失效 + 无法去重



------





### **❌ 错误 2：去重写错位置**



```
if nums[left] == nums[left + 1]: left += 1  # ❌ 只跳一次
```

👉 必须用 while



------





### **❌ 错误 3：没做剪枝**



```
if nums[i] > 0: break
```

👉 会做很多无效计算



------





## **九、面试标准回答模板**





> 我先对数组进行排序，然后固定一个数，使用双指针在其右侧查找另外两个数，使三数之和为 0。通过在固定元素和双指针移动过程中跳过重复值，保证结果中不包含重复三元组，时间复杂度为 O(n²)。



------







看到题目出现：



- 三数之和 / 四数之和
- 去重
- 不要求下标，只要组合





👉 **排序 + 双指针**

