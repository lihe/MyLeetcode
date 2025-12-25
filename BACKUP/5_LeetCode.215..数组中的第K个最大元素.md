# [LeetCode 215. 数组中的第K个最大元素](https://github.com/lihe/MyLeetcode/issues/5)


https://leetcode.cn/problems/kth-largest-element-in-an-array/solutions/821137/ji-yu-kuai-pai-de-suo-you-topkwen-ti-jia-ylsd/






------





## **一、TopK 问题的本质**





> **TopK = Partition（切分） + 选择（不完全排序）**



不是排序问题，

而是 **“把第 k 个元素放到它最终该在的位置”**。



------





## **二、面试中 TopK 的 5 种标准问法**



| **问法**           | **本质**               |
| ------------------ | ---------------------- |
| 前 k 小的数        | 切到索引 k-1           |
| 第 k 小的数        | 找索引 k-1             |
| 前 k 大的数        | 切到索引 n-k           |
| 第 k 大的数        | 找索引 n-k             |
| 只排序前 / 后 k 个 | Quickselect + 局部排序 |

👉 **90% 的 TopK 都能映射到这 5 个**



------





## **三、两种主流解法选型**







### **1️⃣ 快选（Quickselect）—— 面试首选**





- 平均时间复杂度：O(n)
- 空间复杂度：O(1)
- 原地
- 不需要完全排序





📌 **当 k 不接近 n，或强调效率 → 一定选它**



------





### **2️⃣ 堆（Heap）—— 稳定保底方案**





- 时间复杂度：O(n log k)
- 适合 **流式数据 / 超大数据 / 在线 TopK**





📌 面试官追问时你可以说：



> 如果是数据流或内存受限，我会用小根堆 / 大根堆。



------





## **四、Partition（核心中的核心）**







### **标准挖坑法**



```python
def partition(nums, left, right):
    pivot = nums[left]
    i, j = left, right
    while i < j:
        while i < j and nums[j] >= pivot:
            j -= 1
        nums[i] = nums[j]
        while i < j and nums[i] <= pivot:
            i += 1
        nums[j] = nums[i]
    nums[i] = pivot
    return i
```

📌 面试官可能追问：



- 这是 Hoare 还是 Lomuto？

  👉 **Hoare 变体（挖坑法）**





------





## **五、Quickselect 模板**





> **k 一定是 0-based 索引**

```python
def quickselect(nums, k, left, right):
    if left < right:
        index = partition(nums, left, right)
        if index == k:
            return
        elif index < k:
            quickselect(nums, k, index + 1, right)
        else:
            quickselect(nums, k, left, index - 1)
```



------





## **六、TopK 全家桶**







### **1️⃣ 前 k 小的数**



```python
def topk_smalls(nums, k):
    quickselect(nums, k - 1, 0, len(nums) - 1)
    return nums[:k]
```



------





### **2️⃣ 第 k 小的数**



```python
def kth_small(nums, k):
    quickselect(nums, k - 1, 0, len(nums) - 1)
    return nums[k - 1]
```



------





### **3️⃣ 前 k 大的数**



```python
def topk_larges(nums, k):
    n = len(nums)
    quickselect(nums, n - k, 0, n - 1)
    return nums[n - k:]
```



------





### **4️⃣ 第 k 大的数**



```python
def kth_large(nums, k):
    n = len(nums)
    quickselect(nums, n - k, 0, n - 1)
    return nums[n - k]
```



------





### **5️⃣ 只排序前 k 个小的数**



```python
def topk_sort_left(nums, k):
    quickselect(nums, k - 1, 0, len(nums) - 1)
    quicksort(nums, 0, k - 1)
    return nums
```



------





### **6️⃣ 只排序后 k 个大的数**



```python
def topk_sort_right(nums, k):
    n = len(nums)
    quickselect(nums, n - k, 0, n - 1)
    quicksort(nums, n - k, n - 1)
    return nums
```



------





## **七、k 的语义速查表**



| **人类语义** | **Quickselect 用的 k** |
| ------------ | ---------------------- |
| 第 k 小      | k-1                    |
| 前 k 小      | k-1                    |
| 第 k 大      | n-k                    |
| 前 k 大      | n-k                    |

📌 **90% TopK bug 来自这里**



------





## **八、面试官高频追问 & 标准回答**







### **Q1：为什么不用快速排序？**





> 因为 TopK 不需要全局有序，只需要第 k 个元素就位。

> Quickselect 平均时间复杂度是 O(n)，优于排序的 O(n log n)。



------





### **Q2：Quickselect 最坏情况？**





> 最坏是 O(n^2)，可以通过随机 pivot 或三数取中优化。



------





### **Q3：什么时候用堆？**





> 数据流、k 很小、或者无法一次性加载全部数据时。



------





### **Q4：TopK 会稳定吗？**





> 不稳定。Quickselect 和 Heap 都不保证稳定性。



------





## **九、30 秒面试答题心法**





> TopK 本质是选择问题而不是排序问题。

> 我会使用基于 Partition 的 Quickselect，在平均 O(n) 时间内把第 k 个元素放到正确位置。

> 如果需要排序前 / 后 k 个元素，只在局部再做一次排序即可。



------



