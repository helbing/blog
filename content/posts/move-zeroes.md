+++
title = "Move Zeroes"
date = 2020-08-27T09:25:30+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given an array `nums`, write a function to move all `0's` to the end of it while maintaining the relative order of the non-zero elements.

Example:

```text
Input: [0,1,0,3,12]
Output: [1,3,12,0,0]
```

Note:

1. You must do this in-place without making a copy of the array.
2. Minimize the total number of operations.

> 题目地址: [Move Zeroes](https://leetcode-cn.com/problems/move-zeroes/)

## 解题思路

这道题看起来没啥边界问题，数据结构也可以直接使用数组，更多还是算法上的考虑，我的想法是直接使用双指针来解决

```text
指针 i 用于遍历数组
指针 j 用于交换数
指针 i 发现当前自己的数不为0，就与指针 j 交换位置，保证非0的数按顺序在左侧
```

代码如下

```go
j := 0

for i:=0; i < len(nums); i++ {
    if nums[i] != 0 {
        nums[j] = nums[i]
        j++
    }
}

for ; j < len(nums); j++ {
    nums[j] = 0
}
```
