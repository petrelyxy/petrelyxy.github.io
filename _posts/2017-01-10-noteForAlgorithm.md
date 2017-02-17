---
layout: post
title:  "算法笔记"
date:   2016-12-30 11:25:36 +0800
categories: RG
---
# 算法笔记  

　　乘着上的算法导论课还有些映象，做一些记录。

* 贪心算法  
* 动态规划  
  * 自顶向下
  * 自底向上
* 分治法  
* 动态规划  
* NP问题，规约  
* 近似算法
* load balancing  
  

<span id="greedy"></span>
## 贪心算法  
*it buids up a solution in small steps,choosing a desicion at each step myopically(目光短浅地) to optimize some unerlying criterion(标准)*  
简而言之：由局部最优--->全局最优.  **有时不存在**  
典型问题：Interval Scheduling  
  * starts first(x)  
  * smallest interval of time (x)  
  * fewest conflicts (x)  
  * accept the request that finishes first  (yes) [O(n*logn)]d
  

## Divide and conquer （分治法）
1. Divide:divide the problem into one or more subproblems
2. Conquer:conquer each subproblems recursively.
3. combine:solution.
eg.* 归并排序，    
   * 二分查找（binary search）  
   * Powering a number(乘方问题)  
   * Fibonacci number(斐波那契数列)：Bottom-up,
   * Matrix:n*n matrix = 2*2 block matrix of n/2*n/2 sub matrices
