+++
title = "Maximum Subarray"
date = 2020-08-24T22:45:04+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

Examples

```text
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

Follow up:

If you have figured out the `O(n)` solution, try coding another solution using the divide and conquer approach, which is more subtle.

> 题目地址: [Maximum Subarray](https://leetcode-cn.com/problems/maximum-subarray/)

## 解题思路

首先确定下边界

```go
if len(nums) == 0 {
    return 0
}

if len(nums) == 1 {
    return nums[0]
}
```

接下来确定数据结构，看情况用数组即可，不需要用到其他的数据结构。最后确定下算法，题目中并没有说明 `len(nums)` 的大小，所以我们不能简单的使用暴力破解的方式得到答案，应当考虑使用 DP。根据题目我们可以知道

```text
dp(0) = nums[0]
dp(1) = max(dp(0)+nums[1], nums[1]) => dp(0) > 0 ? dp(0)+nums[1] : nums[1]
...
dp(i) = max(dp(i-1)+nums[i], nums[i]) => dp(i-1) > 0 ? dp(i-1)+nums[i] : nums[i]
```

我们可以得到代码，如下

```go
dp := make([]int, len(nums))
dp[0] = nums[0]

max := dp[0]

for i := 1; i < len(dp); i++ {
    if dp[i-1] > 0 {
        dp[i] = dp[i-1] + nums[i]
    } else {
        dp[i] = nums[i]
    }

    if dp[i] > max {
        max = dp[i]
    }
}
```

其实我们还可以再次进行简化，`dp` 这个变量其实是不需要用到的，可以省点内存空间，虽然可读性变弱了很多，如下

```go
max := nums[0]

for i := 1; i < len(nums); i++ {
    if nums[i-1] > 0 {
        nums[i] += nums[i-1]
    }

    if nums[i] > max {
        max = nums[i]
    }
}
```

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/array/maximum_subarray.go)
