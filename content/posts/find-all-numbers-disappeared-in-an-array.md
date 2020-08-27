+++
title = "Find All Numbers Disappeared in an Array"
date = 2020-08-27T23:47:02+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given an array of integers where `1 ≤ a[i] ≤ n` (n = size of array), some elements appear twice and others appear once.

Find all the elements of `[1, n]`inclusive that do not appear in this array.

Could you do it without extra space and in `O(n)` runtime? You may assume the returned list does not count as extra space.

Example:

```text
Input:
[4,3,2,7,8,2,3,1]

Output:
[5,6]
```

> 题目地址: [Find All Numbers Disappeared in an Array](https://leetcode-cn.com/problems/find-all-numbers-disappeared-in-an-array/)

## 解题思路

看起来没有边界问题，而且题目说明了不允许使用额外的空间。最主要还是算法问题，我们采用原地**原地修改**的方式

```text
[4,3,2,7,8,2,3,1] # 原数组，下标从1开始
[4,3,2,-7,8,2,3,1] # 将下标abs(4)的值修改为负数
[4,3,-2,-7,8,2,3,1] # 将下标abs(3)的值修改为负数
[4,-3,-2,-7,8,2,3,1] # 将下标abs(-2)的值修改为负数
[4,-3,-2,-7,8,2,3,1] # 将下标abs(-2)的值修改为负数
[4,-3,-2,-7,8,2,3,-1] # 将下标abs(-7)的值修改为负数
[4,-3,-2,-7,-8,2,3,-1] # 将下标abs(8)的值修改为负数
[4,-3,-2,-7,-8,2,3,-1] # 将下标abs(8)的值修改为负数
[4,-3,-2,-7,-8,2,3,-1] # 将下标abs(2)的值修改为负数
[4,-3,-2,-7,-8,2,3,-1] # 将下标abs(3)的值修改为负数
[-4,-3,-2,-7,-8,2,3,-1] # 将下标abs(-1)的值修改为负数
```

可以看到下标为5和6为正数，那么就意味着原数组中是没有它们的，也就是这道题的答案

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/array/find_all_numbers_disappeared_in_an_array.go)
