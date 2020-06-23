---
title: 串的模式匹配KMP算法
date: 2020-03-25 17:27:48
tags:
    - 字符串
categories:
        - 算法
---
# KMP算法

复杂度为O(n+m)，美团一面的时候面试官问的给定两个字符串S_1,S_2计算S_2在S_1中出现的次数。当时由于S_2就只有2位,我就没往KMP上想，直接一次循环写了，面试官当时说了句这个例子不好，确实。然后问我怎么优化，我没回答上来。

这里写一下KMP的Python实现

```python
# 串的模式匹配KMP算法

class KMP:
    def __init__(self, string, pattern):
        self._match = self._buildMatch(pattern)
        self.counts = self._count(string, pattern)

    def _buildMatch(self, pattern: str) -> list:
        match = [-1]
        for j in range(1, len(pattern)):
            i = match[j - 1]
            while pattern[i + 1] != pattern[j] and i >= 0:  # 类似递归查找
                i = match[i]
            if pattern[i + 1] == pattern[j]:
                match.append(i + 1)
            else:
                match.append(-1)
        return match

    # 查找
    def _kmp(self, string: str, pattern: str) -> int:
        s = p = 0
        while s < len(string) and p < len(pattern):
            if string[s] == pattern[p]:
                s += 1
                p += 1
            elif p > 0:
                p = self._match[p - 1] + 1
            else:  # p指向0
                s += 1
        return s - len(pattern) if p == len(pattern) else -1

    # 计数
    def _count(self, string: str, pattern: str) -> int:
        start = 0
        count = 0
        while True:
            idx = self._kmp(string[start:], pattern)
            if idx != -1:
                count += 1
                start += idx + 1
            else:
                break
        return count

if __name__ == "__main__":
    string = "This is a simple example"
    pattern = "is"
    k = KMP(string, pattern)
    print(k.counts)


```

