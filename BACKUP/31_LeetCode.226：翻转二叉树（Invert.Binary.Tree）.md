# [LeetCode 226：翻转二叉树（Invert Binary Tree）](https://github.com/lihe/MyLeetcode/issues/31)

[226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)


题目的意思是：



> **把每个节点的左右子树交换。**



------





# **一、题目直观理解**





原树：

```
      4
     / \
    2   7
   / \ / \
  1  3 6  9
```

翻转后：

```
      4
     / \
    7   2
   / \ / \
  9  6 3  1
```

规律：

```
每个节点：
left ↔ right
```



------





# **二、核心思路**





对于每个节点：

```
交换它的左右孩子
```

然后继续处理：

```
左子树
右子树
```

这非常适合 **递归**。



------





# **三、递归解法（最经典）**



```python
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root is None:
            return None
        
        # 交换左右子树
        root.left, root.right = root.right, root.left
        
        # 递归处理左右子树
        self.invertTree(root.left)
        self.invertTree(root.right)
        
        return root
```



------





# **四、代码逐行解释**







## **1 递归终止条件**



```
if root is None:
    return None
```

如果节点为空：

```
不用翻转
```



------





## **2 交换左右子树**



```
root.left, root.right = root.right, root.left
```

例如：

```
    4
   / \
  2   7
```

变成：

```
    4
   / \
  7   2
```



------





## **3 递归翻转子树**



```
self.invertTree(root.left)
self.invertTree(root.right)
```

让左右子树也做同样操作。



------





# **五、递归执行过程**





例如：

```
      4
     / \
    2   7
```

步骤：





### **① 翻转 4**



```
      4
     / \
    7   2
```



------





### **② 翻转 7**



```
    7
   / \
  9   6
```

变成

```
    7
   / \
  6   9
```



------





### **③ 翻转 2**



```
    2
   / \
  1   3
```

变成

```
    2
   / \
  3   1
```



------



最终：

```
      4
     / \
    7   2
   / \ / \
  9  6 3  1
```



------





# **六、时间复杂度**



```
O(n)
```

因为：

```
每个节点访问一次
```



------





# **七、空间复杂度**



```
O(h)
```

h 是树高度。



最坏情况：

```
O(n)
```



------





# **八、迭代解法（面试加分）**





可以用 **BFS（队列）**：

```python
from collections import deque

class Solution:
    def invertTree(self, root):
        if not root:
            return None
        
        queue = deque([root])
        
        while queue:
            node = queue.popleft()
            
            node.left, node.right = node.right, node.left
            
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
        
        return root
```



------





# **九、面试回答模板**





可以这样说：



> 这题可以用递归解决。对于每个节点交换左右子树，然后递归处理左右孩子。时间复杂度 O(n)，因为每个节点访问一次。



------





# **十、为什么这题这么经典？**





这题考察：

```
二叉树递归模板
```

很多题都能套这个结构：

```
1 递归终止条件
2 处理当前节点
3 递归左右子树
```



------





# **十一、这题有个很有名的故事**





这题是 Google 面试题。



有人说：



> 如果你无法在白板上写出这题，

> 那你不应该写递归代码。



所以它成了 **递归入门经典题**。



------



如果你愿意，我可以顺便给你讲：



**二叉树面试最重要的 5 个模板**



学会以后：

```
90% 二叉树题
都能秒解
```