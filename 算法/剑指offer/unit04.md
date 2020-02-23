## 面试题14- II. 剪绳子 II

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

 
```
示例 1：

输入: 2
输出: 1
解释: 2 = 1 + 1, 1 × 1 = 1
示例 2:

输入: 10
输出: 36
解释: 10 = 3 + 3 + 4, 3 × 3 × 4 = 36

```

[LeetCode](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof)

### 思路
这题与前面一道的区别主要是数据变大了使用python就不需要考虑越界问题，这里主要考虑如何减少计算量，之前选择动态规划来做，现在选择用贪心，具体分析见这篇[文章](https://leetcode-cn.com/problems/jian-sheng-zi-ii-lcof/solution/javatan-xin-si-lu-jiang-jie-by-henrylee4/)

### 代码

```
class Solution(object):
    def cuttingRope(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n<4:
            return n-1
        x,y=divmod(n,3)
        if y==1:
            x-=1
            y=4
        elif y==0:
            y=1
        res=(pow(3,x)*y) % 1000000007
        return res

        

```

## 面试题15. 二进制中1的个数

请实现一个函数，输入一个整数，输出该数二进制表示中 1 的个数。例如，把 9 表示成二进制是 1001，有 2 位是 1。因此，如果输入 9，则该函数输出 2。
```
示例 1：

输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
示例 2：

输入：00000000000000000000000010000000
输出：1
解释：输入的二进制串 00000000000000000000000010000000 中，共有一位为 '1'。
```
[LeetCode](https://leetcode-cn.com/problems/er-jin-zhi-zhong-1de-ge-shu-lcof)

### 思路
这题主要就是考虑二进制位移运算与运算

### 代码

```
class Solution(object):
    def hammingWeight(self, n):
        """
        :type n: int
        :rtype: int
        """
        res=0
        while n:
            if n & 1:
                res+=1
            n=n>>1
        return res


```

## 面试题16. 数值的整数次方
实现函数double Power(double base, int exponent)，求base的exponent次方。不得使用库函数，同时不需要考虑大数问题。
```
示例 1:

输入: 2.00000, 10
输出: 1024.00000
示例 2:

输入: 2.10000, 3
输出: 9.26100

```

[LeetCode](https://leetcode-cn.com/problems/shu-zhi-de-zheng-shu-ci-fang-lcof)

### 思路
快速幂方法具体见 [文章](https://oi-wiki.org/math/quick-pow/)

### 代码
```
class Solution(object):
    def myPow(self, x, n):
        """
        :type x: float
        :type n: int
        :rtype: float
        """
        if n<0:
            n=-n
            x=1/x
        res=1.0
        while n:
            if n & 1:
                res*=x
            x*=x
            n=n>>1
        return res

```

