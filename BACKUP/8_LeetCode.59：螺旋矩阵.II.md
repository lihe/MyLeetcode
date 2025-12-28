# [LeetCode 59：螺旋矩阵 II](https://github.com/lihe/MyLeetcode/issues/8)


https://leetcode.cn/problems/spiral-matrix-ii/


## **一、核心思路**





> 用 **四个边界** 控制方向：

> 上 → 右 → 下 → 左

> 每走完一条边，就把对应边界往里收缩一圈。



------





## **二、为什么这是最稳的解法？**





- n ≤ 20，规模很小
- 只需要 **模拟填数**
- 不需要复杂数据结构
- 时间复杂度 **O(n²)**（刚好填满矩阵）





------





## **三、标准解法**



```python 
class Solution:
    def generateMatrix(self, n):
        matrix = [[0] * n for _ in range(n)]

        top, bottom = 0, n - 1
        left, right = 0, n - 1
        num = 1

        while top <= bottom and left <= right:
            # 1. 从左到右
            for col in range(left, right + 1):
                matrix[top][col] = num
                num += 1
            top += 1

            # 2. 从上到下
            for row in range(top, bottom + 1):
                matrix[row][right] = num
                num += 1
            right -= 1

            # 3. 从右到左
            if top <= bottom:
                for col in range(right, left - 1, -1):
                    matrix[bottom][col] = num
                    num += 1
                bottom -= 1

            # 4. 从下到上
            if left <= right:
                for row in range(bottom, top - 1, -1):
                    matrix[row][left] = num
                    num += 1
                left += 1

        return matrix
```



------





## **四、手动推演**







### **示例：n = 3**





初始：

```
top=0 bottom=2 left=0 right=2
num=1
```



### **第一圈：**





1️⃣ 左 → 右（top=0）

```
[1, 2, 3]
[0, 0, 0]
[0, 0, 0]
```

2️⃣ 上 → 下（right=2）

```
[1, 2, 3]
[0, 0, 4]
[0, 0, 5]
```

3️⃣ 右 → 左（bottom=2）

```
[1, 2, 3]
[0, 0, 4]
[7, 6, 5]
```

4️⃣ 下 → 上（left=0）

```
[1, 2, 3]
[8, 0, 4]
[7, 6, 5]
```



------





### **第二圈：**



```
top=1 bottom=1 left=1 right=1
```

填中间：

```
[1, 2, 3]
[8, 9, 4]
[7, 6, 5]
```



------





## **五、边界判断为什么不能省？**





这两句是**防止重复填 / 越界**的关键：

```
if top <= bottom:
if left <= right:
```



### **否则会发生什么？**





- n 为奇数时
- 中心点会被重复填
- 或方向走“反了”





👉 **这是本题最常见的翻车点**



------





## **六、时间 & 空间复杂度**





- 时间复杂度：O(n²)
- 空间复杂度：O(n²)（输出矩阵）





这是 **最优 & 不可避免的**。



------





## **七、另一种写法（方向数组法，了解即可）**





思路：

用方向数组 [(0,1),(1,0),(0,-1),(-1,0)] 控制方向

碰壁或已访问就转向



⚠️ 写法更灵活，但更容易出 bug

👉 **面试不如边界法稳**



------





## **八、常见错误总结**







### **❌ 错误 1：忘记边界判断**



```
for col in range(right, left - 1, -1):  # 没判断
```



------





### **❌ 错误 2：边界更新顺序错**





- top += 1、right -= 1 写错位置
- 会导致覆盖 / 漏填





------





### **❌ 错误 3：while 条件写错**



```
while top < bottom and left < right:  # ❌
```

👉 会漏掉中间一圈



------





## **九、面试标准回答模板**





> 我通过维护上下左右四个边界来模拟螺旋填充，每次按照右、下、左、上的顺序填数，并在填完一条边后收缩对应边界，直到所有元素填完。



------





