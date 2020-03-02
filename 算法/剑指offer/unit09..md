## 面试题31. 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

 
```
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
```
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

```
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

```
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