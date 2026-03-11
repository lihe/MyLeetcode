# [LeetCode 221：最大正方形](https://github.com/lihe/MyLeetcode/issues/32)

[221. 最大正方形](https://leetcode.cn/problems/maximal-square/)


它的关键不是暴力枚举边长，而是理解这句转移：



> **以 (i,j) 为右下角的最大正方形边长，取决于它上、左、左上三个位置。**



------





# **一、先说结论：这是 DP 题**





我们定义：

```
dp[i][j]
```

表示：



> **以 matrix[i][j] 这个格子作为右下角，能形成的最大全 1 正方形的边长**



最后答案是：

```
max_side * max_side
```

因为题目要的是 **面积**。



------





# **二、为什么这样定义？**





假设当前位置是 "1"，想让它成为一个更大的正方形右下角，那么它的：



- 上边
- 左边
- 左上角





也必须都能支撑起正方形。



例如：

```
1 1
1 1
```

右下角这个 1 想组成边长为 2 的正方形，必须满足：



- 上面有 1
- 左边有 1
- 左上也有 1





再大一点也是同理。



------





# **三、状态转移公式**





如果：

```
matrix[i][j] == "1"
```

那么：

```
dp[i][j] = min(
    dp[i-1][j],    # 上
    dp[i][j-1],    # 左
    dp[i-1][j-1]   # 左上
) + 1
```

如果：

```
matrix[i][j] == "0"
```

那么：

```
dp[i][j] = 0
```



------





# **四、为什么取min(...) + 1？**



这是这题最核心的地方。



假设：



- 上边最大边长 = 3
- 左边最大边长 = 2
- 左上最大边长 = 3





那当前位置最多只能扩成：

```
min(3,2,3) + 1 = 3
```

因为左边只能提供边长 2 的支撑，再大就断了。



所以必须取最小值。



------





# **五、标准代码**



```python
class Solution:
    def maximalSquare(self, matrix):
        if not matrix or not matrix[0]:
            return 0

        m, n = len(matrix), len(matrix[0])
        dp = [[0] * n for _ in range(m)]
        max_side = 0

        for i in range(m):
            for j in range(n):
                if matrix[i][j] == "1":
                    if i == 0 or j == 0:
                        dp[i][j] = 1
                    else:
                        dp[i][j] = min(
                            dp[i - 1][j],
                            dp[i][j - 1],
                            dp[i - 1][j - 1]
                        ) + 1
                    max_side = max(max_side, dp[i][j])

        return max_side * max_side
```



------





# **六、手动走一个例子**





输入：

```
matrix = [
    ["1","0","1","0","0"],
    ["1","0","1","1","1"],
    ["1","1","1","1","1"],
    ["1","0","0","1","0"]
]
```

对应的 dp 会逐步变成：

```
1 0 1 0 0
1 0 1 1 1
1 1 1 2 2
1 0 0 1 0
```

这里最大的边长是：

```
2
```

所以面积是：

```
2 * 2 = 4
```



------





# **七、边界为什么单独处理？**





当 i == 0 或 j == 0 时，当前位置没有“上、左、左上”三个完整参考。



所以如果它本身是 "1"，最多只能形成边长 1 的正方形：

```
dp[i][j] = 1
```



------





# **八、时间复杂度**





矩阵大小是 m × n：



- 时间复杂度：O(mn)
- 空间复杂度：O(mn)





------





# **九、空间优化（进阶）**





因为 dp[i][j] 只依赖：



- 当前行左边
- 上一行
- 上一行左上





所以可以优化到一维 DP。





### **一维优化版**



```python
class Solution:
    def maximalSquare(self, matrix):
        if not matrix or not matrix[0]:
            return 0

        m, n = len(matrix), len(matrix[0])
        dp = [0] * (n + 1)
        max_side = 0
        prev = 0  # 表示左上角 dp[i-1][j-1]

        for i in range(1, m + 1):
            prev = 0
            for j in range(1, n + 1):
                temp = dp[j]
                if matrix[i - 1][j - 1] == "1":
                    dp[j] = min(dp[j], dp[j - 1], prev) + 1
                    max_side = max(max_side, dp[j])
                else:
                    dp[j] = 0
                prev = temp

        return max_side * max_side
```



------





# **十、面试回答模板**





你可以这样说：



> 这题用动态规划。定义 dp[i][j] 表示以 (i,j) 为右下角的最大全 1 正方形边长。如果当前位置是 1，那么它能扩成多大，取决于上、左、左上三个位置的最小边长，所以状态转移是 min(上, 左, 左上) + 1。最终取最大边长的平方作为答案。



------





# **十一、一句话记忆**





> **最大正方形 = 以当前点为右下角，看上、左、左上的最短边能不能一起扩一格。**



如果你愿意，我还可以继续帮你讲这题为什么不能用“看到 1 就往外暴力扩”的方法，以及它和 **最长递增子序列 / 最大矩形** 的区别。