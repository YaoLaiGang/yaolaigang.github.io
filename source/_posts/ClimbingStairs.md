---
title: ClimbingStairs
date: 2019-05-25 21:57:17
tags: DP
categories: leetcode
---

### 问题描述
这道题是easy题：  
You are climbing a stair case. It takes n steps to reach to the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?  
1. example1:
Input: 2  
Output: 2  
Explanation:  
    1. 1 step + 1 step
    2. 2 steps
2. example2:  
Input: 3  
Output: 3  
Explanation:  
    1. 1 step + 1 step + 1 step
    2. 1 step + 2 steps
    3. 2 steps + 1 step

### 思路
这个就很简单了, 典型的计数型动态规划问题，常规做法：  
记dp[n] 表示到高度n有dp[n]种方法，则dp[n] = dp[n-1]+dp[n-2]  
边界：  
dp[0] = 1 dp[1] = 2  
计算顺序：从左到右  

### 代码
```java
public int climbStairs(int n) {
        int[] dp = new int[n];
        dp[0] = 1;
        if (n>1) dp[1] = 2;
        if (n>2){
            for (int i = 2; i < n; i++) {
                dp[i] = dp[i-1]+dp[i-2];
            }
        }
        return dp[n-1];
    }
```