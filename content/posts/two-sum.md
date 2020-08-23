+++
title = "Two Sum"
date = 2020-08-23T17:31:31+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given an array of integers, return **indices** of the two numbers such that they add up to a specific target.

You may assume that each input would have exactly one solution， and you may not use the same element twice.

Examples

```text
Given nums = [2, 7, 11, 15], target = 9,

Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].
```

> 题目地址: [Two Sum](https://leetcode-cn.com/problems/two-sum/)

## 解题思路

首先确定下边界，可以知道 `len(nums)`一定是大于 2 的，那么就有如下代码

```go
if len(nums) < 2 {
    return []int{}
}
```

接下来确定下使用的数据结构，这里我们可以用 `map` ，`key/value => num/index`

```text
result = map[num]index, map[target-num]index
```

最后确定下使用的算法

```go
for index, num := range nums {
    if m[num] == nil {
        m[target-num] = index
    } else {
        return []int{m[num].(int), index}
    }
}
```

用一层循环进行遍历，`map[num]`存储的 `index` 实际上是 `map[target-num]`的 `index`，所以当 `map[num]`不等于 `nil`，那么也就有结果了

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/algorithms/go/two_sum/two_sum.go)
