# 题目 5: [最长回文子串](https://leetcode.com/problems/longest-palindromic-substring/)
- **难度**: 中等
- **标签**: 动态规划、字符串

## 题目描述
给你一个字符串 `s`，请你找出其中最长的回文子串。

### 示例:
- 输入: `"babad"`
- 输出: `"bab"`
  注意: `"aba"` 也是一个有效答案。

- 输入: `"cbbd"`
- 输出: `"bb"`

## 解题思路
### 方法 1: 动态规划
- 通过动态规划来判断所有子串是否为回文。
- 定义 `dp[i][j]` 表示 `s[i..j]` 是否是回文。
- 边界：`dp[i][i] = True`，`dp[i][i+1]` 根据 `s[i] == s[i+1]` 判断。
- 状态转移：`dp[i][j] = (s[i] == s[j]) and dp[i+1][j-1]`。
- 在遍历过程中维护最长回文的起始位置和长度。

### 方法 2: 中心扩展法
- 回文串的中心可以是一个字符，也可以是两个字符之间。
- 对于每一个可能的回文中心，扩展两边判断回文的长度，找到最长回文。

## 代码

### 动态规划解法
```python
def longestPalindrome(s: str) -> str:
    n = len(s)
    dp = [[False] * n for _ in range(n)]
    start = 0  # 记录回文串的起始位置
    max_len = 1  # 最长回文串的长度
    
    for i in range(n):
        dp[i][i] = True  # 单个字符肯定是回文
    
    for length in range(2, n + 1):  # 子串长度从2到n
        for i in range(n - length + 1):
            j = i + length - 1
            if s[i] == s[j]:
                if length == 2 or dp[i + 1][j - 1]:
                    dp[i][j] = True
                    if length > max_len:
                        max_len = length
                        start = i
    
    return s[start:start + max_len]
