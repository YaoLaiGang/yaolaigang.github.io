---
title: Set Matrix Zeroes
date: 2019-05-27 19:53:04
tags: Matrix
categories: leetcode
---

## 问题描述

 给矩阵置0，就是找矩阵中为0的元素，将其同行同列置0,必须是就地算法（in-place）  
<!--more-->
1. Example 1:
    - input:  

        [
            [1,1,1],
            [1,0,1],
            [1,1,1]
        ]
    - output:  

        [
            [1,0,1],
            [0,0,0],
            [1,0,1]
        ]


2. Example 2:  
    - input:  

        [
            [0,1,2,0],
            [3,4,5,2],
            [1,3,1,5]
        ]  
    - output:  

        [
            [0,0,0,0],
            [0,4,5,0],
            [0,3,1,0]
        ]  

## 整体思路

### 思路1.0版本，朴素想法

最朴素想法，先找为0的坐标，然后将其同行同列置0。这里有个小trick，找到为0的地方不能立即置0，下侧和右侧会导致判断失误。

### 代码1.0

```java
    private void setZeroes(int[][] matrix) {
        //最朴素想法，先找，然后置0
        //注意：置0不能影响后面
        HashSet<Integer> row = new HashSet<>(), col = new HashSet<>();
        if (matrix.length==0) return;
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j]==0){// 填充0
                    row.add(i);
                    col.add(j);
                }
            }
        }
//        置0
        for (int i :
                row) {
            for (int k = 0; k < n; k++) matrix[i][k] = 0;
        }
        for (int j :
                col) {
            for (int k = 0; k < m; k++){
                matrix[k][j] = 0;
            }
        }
        for (int[] nums :
                matrix) {
            System.out.println(Arrays.toString(nums));
        }
    }
```

### 思路2.0版本

上个版本肯定是不work的啦，毕竟最朴素想法时空只能打败30%左右的样子。作为一个**never setter**的人，怎么能容忍这么高的时空复杂度。  
![never settle](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1559564397&di=30cb28ffdd1897d3444284a8a587bc33&imgtype=jpg&er=1&src=http%3A%2F%2Fstatic.oneplus.cn%2Fdata%2Fattachment%2Fforum%2F201411%2F09%2F144717fimzeep1hynehbmr.png)  
上一种方法空间复杂度为O(m\*n),我想办法降到O(1)。注意到当检查到matrix[i][j] == 0 ,不能直接所有行 列置0的原因是会影响下侧和右侧的判断。但是上侧和左侧不会影响，故我们不再使用HashMap，直接将matrix[i][0] = matrix[0][j] = 0 ,然后再检查一下行列开头即可。注意如果本来第0行或者第0列就有0，需要用一个flag来记忆一下，然后再判定置0。  

### 代码2.0

```java
private void setZeroes2(int[][] matrix){
        int m = matrix.length, n = matrix[0].length;
        boolean isCol = false, isRow = false;
        for (int k = 0; k<m; ++k){
            if (matrix[k][0] == 0) {
                isCol = true; // 第一列应当置0
                break;
            }
        }
        for (int k = 0; k<n; ++k){
            if (matrix[0][k] == 0){
                isRow = true; // 第一行应当置0
                break;
            }
        }
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                if (matrix[i][j]==0){// 填充0
                    matrix[i][0] = 0;
                    matrix[0][j] = 0;
                }
            }
        }
        for (int i = 1; i<m; i++){
            if (matrix[i][0] == 0){
                for (int j = 1; j<n; ++j) matrix[i][j] = 0;
            }
        }
        for (int j = 1; j<n; j++){
            if (matrix[0][j] == 0){
                for (int i = 1; i<m; ++i) matrix[i][j] = 0;
            }
        }

        if (isRow)   for (int k = 0; k<n; ++k) matrix[0][k] = 0;
        if (isCol)   for (int k = 0; k<m; ++k) matrix[k][0] = 0;

        for (int[] nums :
                matrix) {
            System.out.println(Arrays.toString(nums));
        }
    }
```

中等题就是这样，解出来比较简单，但是想要拿个top还是比较难的。不管怎样，第二种解法也是top 98%的存在。那么就来个九转大肠鼓励一下自己吧！  
![九转大肠](https://ss3.bdstatic.com/70cFv8Sh_Q1YnxGkpoWK1HF6hhy/it/u=3477725904,3462716497&fm=26&gp=0.jpg)