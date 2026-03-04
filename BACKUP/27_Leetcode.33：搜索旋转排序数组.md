# [Leetcode 33：搜索旋转排序数组](https://github.com/lihe/MyLeetcode/issues/27)

[33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)



# **一、先理解什么是旋转数组**





原始数组是升序：

```
[0,1,2,4,5,6,7]
```

在某个位置 k 左旋：

```
[4,5,6,7,0,1,2]
```

特点：

```
左边一段有序
右边一段有序
中间断开
```

图示：

```
4 5 6 7 | 0 1 2
```



------





# **二、为什么还能用二分查找？**





关键结论：



> **每次二分时，一定有一半是有序的**



因为旋转只发生一次。



所以我们可以：



1️⃣ 判断哪一半有序

2️⃣ 判断 target 是否在该有序区间

3️⃣ 决定往哪边继续二分



------





# **三、核心算法步骤**





初始化：

```
left = 0
right = n-1
```

循环：

```
while left <= right
```



------





## **第一步：找到中点**



```
mid = (left + right) // 2
```

如果：

```
nums[mid] == target
```

直接返回。



------





## **第二步：判断哪一半有序**





关键判断：

```
nums[left] <= nums[mid]
```

成立说明：

```
左半部分有序
```

否则：

```
右半部分有序
```



------





# **四、情况分析**







## **情况1：左半部分有序**



```
nums[left] <= nums[mid]
```

数组结构：

```
left ------ mid | 右半部分
```

判断 target 是否在左半区：

```
nums[left] <= target < nums[mid]
```

如果在：

```
right = mid - 1
```

否则：

```
left = mid + 1
```



------





## **情况2：右半部分有序**



```
nums[left] > nums[mid]
```

结构：

```
左半部分 | mid ----- right
```

判断 target 是否在右区：

```
nums[mid] < target <= nums[right]
```

如果在：

```
left = mid + 1
```

否则：

```
right = mid - 1
```



------





# **五、完整代码（面试标准版）**



```python
class Solution:
    def search(self, nums, target):
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = (left + right) // 2

            if nums[mid] == target:
                return mid

            # 左半部分有序
            if nums[left] <= nums[mid]:
                if nums[left] <= target < nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # 右半部分有序
            else:
                if nums[mid] < target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1

        return -1
```



------





# **六、举例走一遍**



```
nums = [4,5,6,7,0,1,2]
target = 0
```

第一次：

```
left=0
right=6
mid=3
nums[mid]=7
```

左半有序：

```
4 ≤ 7
```

target 是否在左区？

```
4 ≤ 0 < 7 ❌
```

去右边：

```
left = 4
```



------



第二次：

```
left=4
right=6
mid=5
nums[mid]=1
```

左半有序：

```
0 ≤ 1
```

target 在左：

```
0 ≤ 0 < 1
right = 4
```



------



第三次：

```
mid=4
nums[mid]=0
```

找到。



------





# **七、时间复杂度**





二分查找：

```
O(log n)
```

空间：

```
O(1)
```



------





# **八、面试回答模板**





面试官问：思路？



你可以这样说：



> 虽然数组经过旋转，但每次二分时一定有一半是有序的。我们先判断哪一半有序，再判断目标值是否在该有序区间，如果在就继续在该区间二分，否则搜索另一半，从而保持 O(log n) 的时间复杂度。



------





# **九、这一题在算法体系里的位置**





这是一个 **旋转数组二分模板题**，同一套路可以解决：

| **题目**                  | **类型**     |
| ------------------------- | ------------ |
| 搜索旋转数组              | LeetCode 33  |
| 搜索旋转数组 II（有重复） | LeetCode 81  |
| 旋转数组最小值            | LeetCode 153 |
| 旋转数组最小值 II         | LeetCode 154 |

其实都是同一个思想。

