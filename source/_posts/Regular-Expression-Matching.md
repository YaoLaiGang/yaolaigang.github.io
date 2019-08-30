---
title: Regular Expression Matching
date: 2019-05-26 20:29:25
tags: DP
categories: leetcode
---
## 问题描述

字符匹配问题  

* 用p来匹配s. p中可能包含. \*
* . 表示匹配任一个单一字符
* \* 表示匹配0到多个前一个字符
* 要求p和s匹配

1. Example 1:
    - Input:  
        > s="aa"  
        > p = "a"
    - Output: **false**
    - Explanation:  
        > "a" does not match the entire string "aa"
2. Example 2:
    - Input:  
        > s="aa"  
        > p = "a*"
    - Output: **true**
    - Explanation:  
        > "'*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa"
3. Example 3:
    - Input:  
        > s="aab"  
        > p = "c\*a\*b"
    - Output: **true**
    - Explanation:  
        > c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".

## 思路
这是一道**非常**困难的题, 曾经害我苦思冥想了好几天。我刚开始甚至没想到居然能用DP解这个问题，后来苦苦求索翻阅leetcode一众大神的解释后才搞明白这个题用DP到底怎么搞  
这是一道true false dp问题, dp[i][j] 表示了s前i个元素和p前j个元素的匹配情况  
分情况讨论：  

1. 如果s[i] == p [j] || p[j] == '.'
这时候完全就看前面的情况 dp[i][j] = dp[i-1][j-1]
2. 如果s[i] != p [j]这个也要分情况  
    - 2.1 如果p[j] == '\*' (ba a\* ab a\*)  
      - 2.1.1 若 p[j-1] != s[i] && p[j-1] != '.'  
        此时, 由于上一个元素不匹配导致\*无法复制，那么\*只能让上一个元素清空:  
        > dp[i][j] = dp[i][j-2]  

      - 2.1.2 除上面的情况外，即*可以复制上一个元素达到匹配的目的，当然也可以不复制上一个元素:  
        > dp[i][j] = dp[i][j-2] (*上一个元素清空*) || dp[i][j-1] (*\*只代表一个元素*) || dp[i-1][j] (*代表多个元素，如果在i前面都能和p匹配，那加一个自然也能匹配*)

    - 2.2 如果p[j] != '*'  
        > dp[i][j] = false

特别解释一下，为什么在2.1.2中，当代表多个元素时，匹配的是dp[i-1][j]这个奇怪的搭配。让我们来举个栗子:  
![举个栗子](https://cn.bing.com/th?id=OIP.KxkJwLabDhEU-WmW2u8LoAHaHQ&pid=Api&rs=1&p=0)
假设我们的有  
> s: abbbbb  
> p: cb*  

我们看到这里的*，实际上是代表了5个b,但是当我们求dp[i][j]的时候，我们无法得匹配完这些b之后前面的元素是否匹配，我们删掉这些b  
> s: a  
> p: c  

也就是说，a,c是否匹配已经在之前迭代了。如何得知a,c的迭代位置呢？  
> dp[i][j] = dp[i-1][j] = …… = dp[i-6][j]

这是通过我们之前已经求到的结果迭代出来的，你会发现i递减的过程其实就是在找重复元素之前的元素，所以我们直接给出了dp[i-1][j]

## 标准代码
其实如果能看懂上面的解释的话，代码不成问题，上面的解释已经接近于伪代码了。

```java
public  boolean isMatch(String s, String p) {
        boolean[][] dp = new boolean[s.length()+1][p.length()+1];
        // s为空, p为空
        dp[0][0] = true;
        // s为空， p不空
        for (int j = 1; j <= p.length(); j++) {
            if (p.charAt(j-1)=='*' && dp[0][j-2]) dp[0][j] = true;
        }
        //s不空，p空，直接默认false
        for (int i = 1; i < s.length() + 1; i++) {
            for (int j = 1; j < p.length() + 1; j++) {
                if (s.charAt(i-1)==p.charAt(j-1) || p.charAt(j-1)=='.') dp[i][j] = dp[i-1][j-1];
                else {
                    if (p.charAt(j-1)=='*'){
                        if (p.charAt(j-2)!=s.charAt(i-1)&&p.charAt(j-2)!='.') dp[i][j] = dp[i][j-2];
                        else dp[i][j] = dp[i][j-2] || dp[i][j-1] || dp[i-1][j];
                    }
                }
            }
        }
        return dp[s.length()][p.length()];
    }
```

终于啃完这个头疼的问题了，看张图片奖励下自己吧
![bing图片](http://pic8.nipic.com/20100702/5155385_015834315161_2.jpg)