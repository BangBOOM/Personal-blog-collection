## 面试题19. 正则表达式匹配
请实现一个函数用来匹配包含'. '和'*'的正则表达式。模式中的字符'.'表示任意一个字符，而'*'表示它前面的字符可以出现任意次（含0次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab*a"均不匹配。
```tex
示例 1:

输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。

```

[LeetCode]
[https://leetcode-cn.com/problems/zheng-ze-biao-da-shi-pi-pei-lcof]

### 思路

这里我考虑的是用计算理论中的自动机的知识来实现

主要就是将p构建成自动机，也可以理解成一种图，然后使用dfs算法进行遍历查找

```tex
Example: s="ab.*de" 
构建如下的状态转换图其中'$'代表空转移

    a        b        $        d        e
1 -----> 2 -----> 3 -----> 4 -----> 5 ----->6
                 / \  
                 \ /
                  .
```


### 代码

```python
class Solution(object):
    def isMatch(self, s, p):
        """
        :type s: str
        :type p: str
        :rtype: bool
        """
```
~~~python
    状态转移表的构建 ↓
    ```
    dic = {}
    state = 1
    for idx, i in enumerate(p):
        if 'a' <= i <= "z" or i == '.':
            dic.setdefault(state, {}).update({i: state + 1})
            state += 1
        elif i == '*':
            dic[state - 1][p[idx - 1]] = state - 1
            dic[state - 1]['$'] = state
    
    ```
    dfs部分
    ```
    def helper(sta, px):
        if px:
            try:
                tmp1 = dic[sta].get(px[0], False)
                tmp2 = dic[sta].get('.', False)
                tmp3 = dic[sta].get('$', False)
            except:
                return False
            if tmp1:
                if helper(tmp1, px[1:]):
                    return True
            if tmp2:
                if helper(tmp2, px[1:]):
                    return True
            if tmp3:
                if helper(tmp3,px):
                    return True
            return False
        else:
            try:
                tmp = dic[sta].get('$', False)
                return helper(tmp,px)
            except:
                pass
            if sta == state:
                return True
            return False
    return helper(1, s)

s = Solution()
tests = [
    ("babbcaacbccbbbbbabb","bb*.b*b*a*aba*c"),
    ("aa","a*"),
    ("abbabaaaaaaacaa", "a*.*b.a.*c*b*a*c*"),
    ("aaba", "ab*a*c*a"),
    ("aaa", "a*a"),
    ("abcdede", "ab.*de"),
]
for item in tests:
    res = s.isMatch(*item)
    print(res)
~~~
