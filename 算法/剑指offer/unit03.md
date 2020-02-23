## 面试题12. 矩阵中的路径

请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。路径可以从矩阵中的任意一格开始，每一步可以在矩阵中向左、右、上、下移动一格。如果一条路径经过了矩阵的某一格，那么该路径不能再次进入该格子。例如，在下面的3×4的矩阵中包含一条字符串“bfce”的路径（路径中的字母用加粗标出）。
```
[["a","b","c","e"],
["s","f","c","s"],
["a","d","e","e"]]

但矩阵中不包含字符串“abfb”的路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入这个格子。

 
示例 1：

输入：board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]], word = "ABCCED"
输出：true
示例 2：

输入：board = [["a","b"],["c","d"]], word = "abcd"
输出：false

```
[LeetCode](https://leetcode-cn.com/problems/ju-zhen-zhong-de-lu-jing-lcof)

### 思路

这是一道路径搜索的题目很直接的DFS，用递归的形式
```
dfs函数的返回类型是bool

dfs(x,y,k)=dfs(x-1,y,k+1) or dfs(x+1,y,k+1) or dfs(x,y-1,k+1) or dfs(x,y+1,k+1)

其中x,y是在矩阵中的坐标,k是字符串中的第几个字母

```
当然我个人觉得这题可以线查找出字符串中的每个字母在矩阵中的位置，在根据这些字母在剧中中的位置构建关系矩阵将这几个点单独取出来看是否存在一条通路

### 代码

```

class Solution(object):
    def exist(self, board, word):
        """
        :type board: List[List[str]]
        :type word: str
        :rtype: bool
        """
        def dfs(x,y,k):
            if k==len(word):
                return True
            if x<0 or x>len(board)-1 or y<0 or y>len(board[0])-1 or board[x][y]!=word[k]:
                return False
            tmp,board[x][y]=board[x][y],None
            res=dfs(x,y+1,k+1) or dfs(x,y-1,k+1) or dfs(x-1,y,k+1) or dfs(x+1,y,k+1)
            board[x][y]=tmp
            return res
        
        for x in range(len(board)):
            for y in range(len(board[0])):
                if board[x][y]==word[0]:
                    if dfs(x,y,0):
                        return True
        return False


```


## 面试题13. 机器人的运动范围

地上有一个m行n列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于k的格子。例如，当k为18时，机器人能够进入方格 [35, 37] ，因为3+5+3+7=18。但它不能进入方格 [35, 38]，因为3+5+3+8=19。请问该机器人能够到达多少个格子？

```

示例 1：

输入：m = 2, n = 3, k = 1
输出：3
示例 1：

输入：m = 3, n = 1, k = 0
输出：1

```
[LeetCode](https://leetcode-cn.com/problemsji-qi-ren-de-yun-dong-fan-wei-lcof)

### 思路

这其实也是一道路径问题，用递归或者是栈加循环实现

写出递归形式
```
path(x,y)=path(x,y+1)+path(x,y-1)+path(x+1,y)+path(x-1,y)
```
根据题目要求再加上三个个限定条件

1. `x,y`在矩阵中
2. `x,y`未访问过，这里用一个集合存放访问过的坐标
3. `x,y`各位和不大于给定的K

### 代码
```
class Solution(object):
    def movingCount(self, m, n, k):
        """
        :type m: int
        :type n: int
        :type k: int
        :rtype: int
        """
        def cal(x,y):
            sum=0
            while x:
                sum+=x%10
                x//=10
            while y:
                sum+=y%10
                y//=10
            return sum<=k;
        
        vist=set()

        def helper(x,y):
            if 0<=x<m and 0<=y<n and (x,y) not in vist:
                if cal(x,y):
                    vist.add((x,y))
                    return 1+helper(x,y+1)+helper(x,y-1)+helper(x+1,y)+helper(x,y-1)
            return 0
        
        return helper(0,0)

```

## 面试题14-1. 剪绳子

给你一根长度为 n 的绳子，请把绳子剪成整数长度的 m 段（m、n都是整数，n>1并且m>1），每段绳子的长度记为 k[0],k[1]...k[m] 。请问 k[0]*k[1]*...*k[m] 可能的最大乘积是多少？例如，当绳子的长度是8时，我们把它剪成长度分别为2、3、3的三段，此时得到的最大乘积是18。
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
[LeetCode](https://leetcode-cn.com/problems/jian-sheng-zi-lcof)


### 思路
这题是典型的动态规划，就是先确定最后一段，穷尽前一部分的最大值，

列出方程
```
res=max(res,i*cuttingRope(n-i),i*(n-i))

其中cuttingRope计算(n-i)线段的最大成绩给这个i是保留段的大小

```

### 代码
```
class Solution(object):

    def __init__(self):
        self.dic={2:1,3:2,4:4}
    def cuttingRope(self, n):
        """
        :type n: int
        :rtype: int
        """
        tmp=self.dic.get(n,False)
        if tmp:
            return tmp
        res=0
        for i in range(2,n-2):
            res=max(res,i*self.cuttingRope(n-i),i*(n-i))
        self.dic[n]=res
        return res
```