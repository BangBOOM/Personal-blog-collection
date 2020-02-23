# 剑指offer系列

> 开始在leetcode上刷剑指offer系列的题目每天完成一些并记录思路

## 面试题03.数组中重复的数字
在一个长度为 n 的数组 nums 里的所有数字都在 0～n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

```
输入：
[2, 3, 1, 0, 2, 5, 3]
输出：2 或 3 
```

[leetcode](https://leetcode-cn.com/problems/shu-zu-zhong-zhong-fu-de-shu-zi-lcof/)

### 思路
其实是一个哈希表的思想，可以使用python中的集合实现依次遍历查找判断值是否在集合中

这种情况可能有人会选择list但是in运算在集合中的速度在元素数量巨大时是非常快的

### 代码
```
class Solution(object):
    def findRepeatNumber(self, nums):
        """
        :type nums: List[int]
        :rtype: int
        """
        s=set()
        for n in nums:
            if n not in s:
                s.add(n)
            else:
                return n
        return None
```

## 面试题04.二维数组查找

在一个 n * m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

示例:现有矩阵 matrix 如下：
```
[
  [1,   4,  7, 11, 15],
  [2,   5,  8, 12, 19],
  [3,   6,  9, 16, 22],
  [10, 13, 14, 17, 24],
  [18, 21, 23, 26, 30]
]
```
给定 target = 5，返回 true。

给定 target = 20，返回 false。

[LeetCode](https://leetcode-cn.com/problemser-wei-shu-zu-zhong-de-cha-zhao-lcof)

### 思路
题目中的关键点是从左到右从上到下递增，这点就需要抓住一些特殊的位置类似二分查找一次可以删除一半，这里根据最左下角的数字就可以判断并去除一整行或一整列。

具体的
+  target > 18 则第一列可以删除
+  target < 18 则最下面一行可以删除

### 代码
```
class Solution(object):
    def findNumberIn2DArray(self, matrix, target):
        """
        :type matrix: List[List[int]]
        :type target: int
        :rtype: bool
        """
        if matrix    
            n=len(matrix)
            m=len(matrix[0])
            i=0
            while i<m and n>-1:
                if target> matrix[n-1][i]:
                    i+=1
                elif target<matrix[n-1][i]:
                    n-=1
                else:
                    return True
        return False
```

## 面试题05.替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

示例 1：
```
输入：s = "We are happy."
输出："We%20are%20happy."
```
[LeetCode](https://leetcode-cn.com/problems/ti-huan-kong-ge-lcof)

### 思路
python中字符串的处理方式非常多，在这里我选择先安装空格拆成字符串，再使用join的方法拼接，不过速度会比较慢,空间消耗比绩小

### 代码
```
class Solution(object):
    def replaceSpace(self, s):
        """
        :type s: str
        :rtype: str
        """
        s="%20".join(s.split(' '))
        return s
```

## 面试题07.重建二叉树
输入某二叉树的前序遍历和中序遍历的结果，请重建该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。

例如，给出
```
前序遍历 preorder = [3,9,20,15,7]
中序遍历 inorder = [9,3,15,20,7]
返回如下的二叉树：


    3
   / \
  9  20
    /  \
   15   7
```
[LeetCode](https://leetcode-cn.com/problems/zhong-jian-er-cha-shu-lcof)

### 思路
前序遍历：root->left->right

中序遍历：left->root->right

以题目给出的示例：

+ 前序遍历中的3是根节点，所以把3从前序遍历的列表弹出后得到新的前序遍历列表[9,20,15,7]
```
  3
```

+ 中序遍历中3的左半部分是左子树，将这部分单独取出即：[9]

    + 前序遍历中的9是左子树部分的根节点，将9从前序遍历的列表弹出[20,15,7]
```
  3
 /
9
```

+ 中序遍历中3的右半部分是右子树，将这部分单独去除进行递归处理[15,20,7]

    + 前序遍历中的20是右子树部分的根节点,将20从前序遍历的列表弹出[15,7]
    + 中序遍历中20的左半部分是左子树[15],15是前序遍历中的第一个，所以15是根节点，同时将15从前序遍历中弹出
    + 中序遍历中20的右半部分是右子树[7]，7是前序遍历中的第一个，所以7是根节点，同时将7从前序遍历中弹出
```
  3                     3                    3
 / \     --->          / \      --->        / \
9  20                 9  20                9  20
                        /                    /  \
                       15                   15   7
```

### 代码
```
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def buildTree(self, preorder, inorder):
        """
        :type preorder: List[int]
        :type inorder: List[int]
        :rtype: TreeNode
        """
        if not preorder or not inorder:
            return None
        rx=preorder.pop(0)
        root=TreeNode(rx)
        index=inorder.index(rx)
        if inorder[0:index]:
            root.left=self.buildTree(preorder,inorder[0:index])
        if inorder[index+1:]:
            root.right=self.buildTree(preorder,inorder[index+1:])
        return root
```


    
    
