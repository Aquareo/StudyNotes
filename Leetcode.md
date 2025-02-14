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
```c++
string longestPalindrome(string s) 
    {   
        int n=s.size();
        int ans=1;

        int start=0;
        int end=0;
        vector<vector<int>>dp(n,vector<int>(n,0));


        //memo[i][j]表示i->j是否回文串
        
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==j)dp[i][j]=1;
                else 
                {
                    if(s[i]==s[j])
                    {
                        if(i+1<n&&j-1>=0)
                        {
                             dp[i][j]=dp[i+1][j-1];
                        }
                        else dp[i][j]=1;
                    }
                    else dp[i][j]=0;


                    cout<<"i= "<<i <<" j= "<<j<<' ' <<dp[i][j]<<endl;
                    if(dp[i][j]==1&&j-i+1>ans)
                    {
                        start=i;
                        end=j;
                    }
                }
            }
        }

        return  s.substr(start, end- start + 1);;
    }
```

### 中心扩展解法


```c++


```
