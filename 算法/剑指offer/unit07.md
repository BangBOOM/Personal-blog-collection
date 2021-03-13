## 面试题26. 树的子结构

输入两棵二叉树A和B，判断B是不是A的子结构。(约定空树不是任意一个树的子结构)

B是A的子结构， 即 A中有出现和B相同的结构和节点值。

```tex
例如:
给定的树 A:

     3
    / \
   4   5
  / \
 1   2
给定的树 B：

   4 
  /
 1
返回 true，因为 B 与 A 的一个子树拥有相同的结构和节点值。

```

[leetcode](https://leetcode-cn.com/problems/shu-de-zi-jie-gou-lcof/)


### 思路

这题是递归和dfs的思想

指针`p1,p2`分别指向两个二叉树的当前节点

若`p1.val==p2.val` 则对 `p1,p2` 进行 `dfs` 判断是否是子结构，终止情况是 `p2.left==p2.right==None` 也就是`p2`完全遍历了。

否则对`p1.left,p1.right`分别`p2`是否是其子结构


### 代码
```python

# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def isSubStructure(self, A, B):
        """
        :type A: TreeNode
        :type B: TreeNode
        :rtype: bool
        """
        if not A or not B:
            return False
        return self.dfs(A,B) or self.isSubStructure(A.left,B) or self.isSubStructure(A.right,B)

    def dfs(self,a,b):
        if not b:
            return True
        if not a:
            return False
        return a.val==b.val and self.dfs(a.left,b.left) and self.dfs(a.right,b.right)
```


### 对递归的一些感悟

递归调用的式子一般是很简洁的，不存在太多条件需要单独考虑，一般条件考虑过多的递归都是规律总结不足造成的。
