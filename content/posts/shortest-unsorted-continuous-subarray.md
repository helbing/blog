+++
title = "Shortest Unsorted Continuous Subarray"
date = 2020-08-28T14:22:58+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given an integer array, you need to find one **continuous subarray** that if you only sort this subarray in ascending order, then the whole array will be sorted in ascending order, too.

You need to find the **shortest** such subarray and output its length.

Example 1:

```text
Input: [2, 6, 4, 8, 10, 9, 15]
Output: 5
Explanation: You need to sort [6, 4, 8, 10, 9] in ascending order to make the whole array sorted in ascending order.
```

Note:

1. Then length of the input array is in range [1, 10,000].
2. The input array may contain duplicates, so ascending order here means `<=`.

> 题目地址: [Shortest Unsorted Continuous Subarray](https://leetcode-cn.com/problems/shortest-unsorted-continuous-subarray/)

## 解题思路

这道题看起来没有边界问题，我也不打算用额外的数据结构。这道题看起来的解法是从左侧开始找出出现降序的下标 `i`，从右侧开始找出出现升序的下标 `j`，那么子数组就是 `[i,j]`。但是在实际中，其实还是有挺多种情况要考虑的，如

```text
input: [1,2,3,4] output: 0
input: [1,3,2,2,2] output: 4
```

所以算法修改为

```go
func findUnsortedSubarray(nums []int) int {
    length := len(nums)

    max := nums[0]
    min := nums[length-1]

    leftIndex := 0
    rightIndex := -1

    for i := 0; i < length; i++ {
        if max > nums[i] {
            rightIndex = i
        } else {
            max = nums[i]
        }

        if min < nums[length-1-i] {
            leftIndex = length - 1 - i
        } else {
             min = nums[length-1-i]
        }
    }

    return rightIndex - leftIndex + 1
}
```

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/array/shortest_unsorted_continuous_subarray.go)
