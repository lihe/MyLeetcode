# [LeetCode 236：二叉树的最近公共祖先](https://github.com/lihe/MyLeetcode/issues/30)


[236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)

  

这是 **LeetCode 236：二叉树的最近公共祖先**，属于**二叉树递归题的代表作**。



这题真正考的不是“找祖先”，而是：



> **你能不能让递归函数返回“这棵子树里是否找到了 p 或 q”**



------





# **一、先给结论（面试可背）**





> 对当前节点 root：



- > 如果 root 是 p 或 q，直接返回 root

- > 分别去左子树、右子树找

- > 如果左右都找到了，说明当前节点就是最近公共祖先

- > 如果只找到一边，就把那一边的结果返回





------





# **二、为什么这样做？**





最近公共祖先的含义是：



- p 和 q 分别在某个节点的左右两边

  **或者**

- 其中一个节点本身就是另一个节点的祖先





所以递归到某个节点时，只需要知道三件事：



1. 左子树有没有找到 p 或 q
2. 右子树有没有找到 p 或 q
3. 当前节点是不是 p 或 q





------





# **三、标准递归解法**



```python
class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        if root is None:
            return None
        
        if root == p or root == q:
            return root

        left = self.lowestCommonAncestor(root.left, p, q)
        right = self.lowestCommonAncestor(root.right, p, q)

        if left and right:
            return root

        return left if left else right
```



------





# **四、逐行解释**







## **1. 递归终止条件**



```
if root is None:
    return None
```

说明这棵子树没找到。



------





## **2. 当前节点就是 p 或 q**



```
if root == p or root == q:
    return root
```

意思是：



> 找到了其中一个目标节点，就把它返回给上层



注意：



- 这里不能继续往下找了
- 因为一个节点也可以是它自己的祖先





------





## **3. 递归左右子树**



```
left = self.lowestCommonAncestor(root.left, p, q)
right = self.lowestCommonAncestor(root.right, p, q)
```

left 和 right 表示：



- 左子树返回了什么
- 右子树返回了什么





返回值可能是：



- None：没找到
- p / q
- 最近公共祖先





------





## **4. 左右都找到了**



```
if left and right:
    return root
```

说明：



- p 在一边
- q 在另一边





所以当前节点 root 就是最近公共祖先。



------





## **5. 只有一边找到**



```
return left if left else right
```

把找到的那一边往上传。



------





# **五、用例子走一遍**





示例：

```
        3
      /   \
     5     1
    / \   / \
   6   2 0   8
      / \
     7   4
```

找：

```
p = 5, q = 1
```



------





## **递归过程**





- 到节点 5：命中，返回 5

- 到节点 1：命中，返回 1

- 回到节点 3：

  

  - left = 5
  - right = 1

  





满足：

```
if left and right:
    return root
```

所以返回：

```
3
```



------





# **六、再看示例 2**





找：

```
p = 5, q = 4
```

过程：



- 到节点 5：命中，返回 5
- 在 5 的右子树里还能找到 4，但因为 5 已经先返回了
- 最后整个递归会把 5 传上去





所以答案：

```
5
```

这也符合：



> 一个节点可以是它自己的祖先



------





# **七、为什么这题适合递归？**





因为它天然符合“后序遍历”逻辑：



- 先问左边找到了什么
- 再问右边找到了什么
- 最后决定当前节点返回什么





这就是典型的：



> **从子问题结果推导父问题结果**



------





# **八、时间复杂度**





- 时间复杂度：O(n)

  

  - 每个节点最多访问一次

  

- 空间复杂度：O(h)

  





------





# **九、面试一句话总结**





> 这题可以用递归解决。递归函数返回当前子树中找到的 p、q 或最近公共祖先。如果左右子树分别返回非空，说明当前节点就是最近公共祖先；否则返回非空的一侧。



------





# **十、你要形成的模板思维**





这题的递归函数本质上是在问：



> **“当前子树里，我能找到什么？”**



很多树题都能套这个思路：



- 最近公共祖先
- 树的直径
- 平衡二叉树
- 路径和





------



如果你愿意，我下一步可以继续帮你讲：



1. **最近公共祖先为什么一定是后序遍历**
2. **二叉搜索树版本的最近公共祖先（更简单）**
3. **把这题画成递归返回值流程图**





你选一个，我继续。
