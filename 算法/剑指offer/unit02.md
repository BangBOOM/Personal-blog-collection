## 面试题09.用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead 操作返回 -1 )

```
示例 1：
输入：
["CQueue","appendTail","deleteHead","deleteHead"]
[[],[3],[],[]]
输出：[null,null,3,-1]
示例 2：

输入：
["CQueue","deleteHead","appendTail","appendTail","deleteHead","deleteHead"]
[[],[],[5],[2],[],[]]
输出：[null,-1,null,null,5,2]
```
[LeetCode](https://leetcode-cn.com/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof)

### 思路

首先理清队列和栈的区别
+ 栈是先进后出，一次只在栈尾添加或弹出一个元素
+ 队列是先进先出，队列的末尾只可以添加元素，队列的头部只可以弹出元素

这样既然题目要求使用两个栈来实现队列的操作就很明显一个栈存放队列的正序，另一个栈存放队列的倒序,具体的实现，我们采用动态的方式

+ 当添加元素即`add(value)`时`stack a`中添加元素，栈b不操作
+ 当弹出元素即`pop()`时若`stack b`不为空则直接弹出其末尾元素，若为空则将`stack a`弹出并存入其中，这样就得到了`stack a`的倒序存放接着再弹出其末尾的元素

```
init:          add(3)    add(4)     pop( )       add(5)    pop()   pop()
stack a []     [3]       [3,4]      []           [5]       [5]     []
stack b []     []        []         [4,3]->[4]   [4]       []      [5]->[]
```

### 代码
```

class CQueue(object):

    def __init__(self):
        self.A,self.B=[],[]
    def appendTail(self, value):
        """
        :type value: int
        :rtype: None
        """
        self.A.append(value)

    def deleteHead(self):
        """
        :rtype: int
        """

        if not self.B:
            while self.A:
                self.B.append(self.A.pop())
        if self.B:
            return self.B.pop()
        else:
            return -1

```

## 面试题10.1 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项。斐波那契数列的定义如下：
```
F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。
```
[LeetCode](https://leetcode-cn.com/problems/fei-bo-na-qi-shu-lie-lcof)

### 思路

斐波那契数列实现的方式有递归和非递归，其中递归比较一目了然但是直接的递归会造成大量的重复计算，所以一般用一种改进的递归方法，即用一个数组记录已经计算过的值，这样就可以避免无意义的递归。当然不用递归也是很容易实现的，直接的每次只要保留最后的两个数字，用python的写法就是
`a,b=b,a+b`

### 代码
```
class Solution(object):
    def fib(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n<1:
            return 0
        a,b=0,1
        for i in range(2,n+1):
            a,b=b,a+b
        return b%1000000007
```


