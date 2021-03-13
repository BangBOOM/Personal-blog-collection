## 面试题25. 合并两个排序的链表

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

```tex
示例1：

输入：1->2->4, 1->3->4
输出：1->1->2->3->4->4

```
[LeetCode](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof)


## 思路

[参考](https://leetcode-cn.com/problems/he-bing-liang-ge-pai-xu-de-lian-biao-lcof/solution/he-bing-liang-ge-pai-xu-de-lian-biao-die-dai-di-gu/)

+ 迭代
  

 两个指针分别指向两个链表的表头，比较大小小的那个节点接到新的链表结尾并向后移动

+ 递归

    
```tex

                                            [1,2,2,3,4]
l1=[1,2,3]
l2=[2,4]
l1.next=merge(l1.next,l2)                   [1]+[2,2,3,4]
↓
l1=[2,3]
l2=[2,4]
l1.next=merge(l1.next,l2)                   [2]+[2,3,4]
↓
l1=[3]
l2=[2,4]
l2.next=merge(l1,l2.next)                   [2]+[3,4]
↓
l1=[3]
l2=[4]
l1.next=merge(l1.next,l2)                   [3]+[4]
↓
l1=[]
l2=[4]
l1==None -->return l2                       [4]

```


## 代码

+ 迭代
```python

# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        res=ListNode(None)
        head=res
        while l1 or l2:
            if not l1:
                res.next=l2
                break
            if not l2:
                res.next=l1
                break
            if l1.val<l2.val:
                res.next=l1
                l1=l1.next
                res=res.next
            else:
                res.next=l2
                l2=l2.next
                res=res.next
        return head.next
                

```

+ 递归

```python
# Definition for singly-linked list.
# class ListNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution(object):
    def mergeTwoLists(self, l1, l2):
        """
        :type l1: ListNode
        :type l2: ListNode
        :rtype: ListNode
        """
        if not l1:
            return l2
        if not l2:
            return l1
        if l1.val<l2.val:
            l1.next=self.mergeTwoLists(l1.next,l2)
            return l1
        else:
            l2.next=self.mergeTwoLists(l1,l2.next)
            return l2
        
```



## 面试题27. 二叉树的镜像

请完成一个函数，输入一个二叉树，该函数输出它的镜像。

```tex
例如输入：

     4
   /   \
  2     7
 / \   / \
1   3 6   9
镜像输出：

     4
   /   \
  7     2
 / \   / \
9   6 3   1
```
[LeetCode](https://leetcode-cn.com/problems/er-cha-shu-de-jing-xiang-lcof)


## 思路

自上而下递归，先交换根节点的左右，再交换根节点的左右节点的左右节点，递归进行

## 代码

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution(object):
    def mirrorTree(self, root):
        """
        :type root: TreeNode
        :rtype: TreeNode
        """
        if root:
            root.left,root.right=root.right,root.left
            if root.left:
                self.mirrorTree(root.left)
            if root.right:
                self.mirrorTree(root.right)
        return root

```