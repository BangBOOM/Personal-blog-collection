## 面试题30. 包含min函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

```tex
示例:

MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.min();   --> 返回 -3.
minStack.pop();
minStack.top();      --> 返回 0.
minStack.min();   --> 返回 -2.
```

[LeetCode](https://leetcode-cn.com/problems/bao-han-minhan-shu-de-zhan-lcof)


### 思路
这题第一反应是用一个常量存放最小值，这才一直存入的情况不会出错，但是弹出数的时候，若弹出的数是最小的，就无法找到第二小的数字。

两个栈一个用于存储正常的数据，一个用于存储**当前的**最小值，保持两个栈存入的数目相同，这样的目的是防止弹出数据时无法确定最小值。

当放入的数字大于等于最小栈栈顶的元素，则把最小栈顶元素的值复制一份，代表当前的最小值未被改变，若放入的数字小于最小栈栈顶的元素，则把这个数同时也放入最小栈中
举个例子： 
```tex
stack:      [-2, 0,-3, 4,-5]
minstack:   [-2,-2,-3,-3,-5]
```

### 代码：
```python
class MinStack(object):

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack=[]
        self.mstack=[]
        

    def push(self, x):
        """
        :type x: int
        :rtype: None
        """
        if not self.stack:
            self.stack.append(x)
            self.mstack.append(x)
        else:
            self.stack.append(x)
            if x>self.mstack[-1]:
                self.mstack.append(self.mstack[-1])
            else:
                self.mstack.append(x)
            
    def pop(self):
        """
        :rtype: None
        """
        self.stack.pop()
        self.mstack.pop()


    def min(self):
        """
        :rtype: int
        """
        return self.mstack[-1]

```

## 面试题28. 对称的二叉树

请实现一个函数，用来判断一棵二叉树是不是对称的。如果一棵二叉树和它的镜像一样，那么它是对称的。

```tex
例如，二叉树 [1,2,2,3,4,4,3] 是对称的。

    1
   / \
  2   2
 / \ / \
3  4 4  3
但是下面这个 [1,2,2,null,3,null,3] 则不是镜像对称的:

    1
   / \
  2   2
   \   \
   3    3



示例 1：

输入：root = [1,2,2,3,4,4,3]
输出：true
示例 2：

输入：root = [1,2,2,null,3,null,3]
输出：false
```
[LeetCode](https://leetcode-cn.com/problems/dui-cheng-de-er-cha-shu-lcof)


### 思路

递归判断左右，注意镜像是从根节点的镜像而不是子树内部镜像

### 代码

```python
class Solution(object):
    def isSymmetric(self, root):
        """
        :type root: TreeNode
        :rtype: bool
        """

        def doseEqual(left,right):
            if left==right:
                return True
            elif not(left and right):
                return False
            if left.val==right.val:
                return doseEqual(left.left,right.right) and doseEqual(left.right,right.left)
            else:
                return False
        if root:
            return doseEqual(root.left,root.right)
        return True
```