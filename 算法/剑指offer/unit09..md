## 面试题31. 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。


```tex
示例 1：

输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
示例 2：

输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
```
[LeetCode](https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof)

### 思路

采用模拟的方式
新建一个栈 `tmp` 

`pushed`中的元素依次放入`tmp`中，同时检测`tmp`的最后一个元素是否与`popped`的第一个元素相同，若相同则从`tmp`中弹出同时`popped`的第一个元素也弹出。最后判断`tmp`与`popped`是否正好相反。

### 代码
```python
class Solution(object):
    def validateStackSequences(self, pushed, popped):
        """
        :type pushed: List[int]
        :type popped: List[int]
        :rtype: bool
        """
        tmp=[]
        for item in pushed:
            tmp.append(item)
            while tmp and popped and tmp[-1]==popped[0]:
                popped.pop(0)
                tmp.pop()
        return tmp[::-1]==popped
```

## 面试题32 - I. 从上到下打印二叉树

从上到下打印出二叉树的每个节点，同一层的节点按照从左到右的顺序打印。

```tex
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回：

[3,9,20,15,7]
```
[LeetCode](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-lcof)

### 思路

树的层序遍历，使用队列存放

### 代码

```python
'''
Definition for a binary tree node.
class TreeNode:
    def __init__(self, x):
        self.val = x
        self.left = None
        self.right = None
'''
class Solution:
    def levelOrder(self, root: TreeNode) -> List[int]:
        que=[root]
        res=[]
        while que:
            tmp=que.pop(0)
            if tmp:
                res.append(tmp.val)
                que.extend([tmp.left,tmp.right])
        return res
```

## 面试题32 - II. 从上到下打印二叉树 II

从上到下按层打印二叉树，同一层的节点按从左到右的顺序打印，每一层打印到一行。
```tex
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [9,20],
  [15,7]
]
```
[LeetCode](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-ii-lcof)

### 思路

直接全部打印不需要记录每层的信息，分层打印需要记录每层都有多少个，或者每次两个队列交替。

### 代码
```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        tmp=[root]
        res=[]
        while tmp:
            res.append([])
            for _ in range(len(tmp)):
                item=tmp.pop(0)
                if item:
                    res[-1].append(item.val)
                    tmp.extend([item.left,item.right])
        res.pop()
        return res
```

## 面试题32 - III. 从上到下打印二叉树 III

请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。
```tex
例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]
```
[LeetCode](https://leetcode-cn.com/problems/cong-shang-dao-xia-da-yin-er-cha-shu-iii-lcof)

### 思路

和分层打印一样，只是将特定层得到的结果倒序输出即可

### 代码

```python
class Solution:
    def levelOrder(self, root: TreeNode) -> List[List[int]]:
        
        direction=False
        tmp=[root]
        res=[]
        while tmp:
            res.append([])
            for _ in range(len(tmp)):
                item=tmp.pop(0)
                if item:
                    res[-1].append(item.val)
                    tmp.extend([item.left,item.right])
            if direction:
                res[-1]=res[-1][::-1]
            direction=not direction
        res.pop()
        return res
```

## 面试题33. 二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历结果。如果是则返回 true，否则返回 false。假设输入的数组的任意两个数字都互不相同。
```tex
参考以下这颗二叉搜索树：

     5
    / \
   2   6
  / \
 1   3
示例 1：

输入: [1,6,3,2,5]
输出: false
示例 2：

输入: [1,3,2,6,5]
输出: true
```

[LeetCode](https://leetcode-cn.com/problems/er-cha-sou-suo-shu-de-hou-xu-bian-li-xu-lie-lcof)

### 思路

根据后续遍历的特点，最后一个点一定是根节点，并且根节点前的数字一定是前一部分小于根节点后一部分大于根节点，我们用递归的形式不断划分

比如
```tex
[1,6,3,2,5]
[1] [6,3,2] 5  find 3<5 so return False

[1,3,4,6,5]
[1,3,4] [6]
[1] [3] True 
True True True
```

### 代码
```python
class Solution:
    def verifyPostorder(self, postorder: List[int]) -> bool:
        if len(postorder)==1 or not postorder:
            return True
        left=[]
        right=[]
        for item in postorder[:-2]:
            if item < postorder[-1]:
                if right:
                    return False
                left.append(item)
            else:
                right.append(item)
        return self.verifyPostorder(left) and self.verifyPostorder(right)
```


## 面试题34. 二叉树中和为某一值的路径

输入一棵二叉树和一个整数，打印出二叉树中节点值的和为输入整数的所有路径。从树的根节点开始往下一直到叶节点所经过的节点形成一条路径。

```tex
示例:
给定如下二叉树，以及目标和 sum = 22，

              5
             / \
            4   8
           /   / \
          11  13  4
         /  \    / \
        7    2  5   1
返回:

[
   [5,4,11,2],
   [5,8,4,5]
]
```
[LeetCode](https://leetcode-cn.com/problems/er-cha-shu-zhong-he-wei-mou-yi-zhi-de-lu-jing-lcof)s

### 思路
深度优先遍历

### 代码
```python
class Solution:
    def pathSum(self, root: TreeNode, sums: int) -> List[List[int]]:
        res_x=[]
        def dfs(res,root):
            res.append(root.val)
            if sum(res)==sums and not root.left and not root.right:
                res_x.append(res)
            if root.left:
                dfs(res.copy(),root.left)
            if root.right:
                dfs(res.copy(),root.right)
        if root:
            dfs([],root)
        return res_x
```