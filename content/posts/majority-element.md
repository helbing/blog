+++
title = "Majority Element"
date = 2020-08-25T22:55:21+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given an array of size *n*, find the majority element. The majority element is the element that appears more than `⌊ n/2 ⌋` times.

You may assume that the array is non-empty and the majority element always exist in the array.

You may assume that the array is non-empty and the majority element always exist in the array.

Example 1:

```text
Input: [3,2,3]
Output: 3
```

Example 2:

```text
Input: [2,2,1,1,1,2,2]
Output: 2
```

> 题目地址: [Majority Element](https://leetcode-cn.com/problems/majority-element/)

## 解题思路

这道题第一反应应该是使用 `map` 来解决，首先确定下边界（题目说了输入的是非空数组，那么就排除了一个边界）

```go
if len(nums) == 1 {
    return nums[0]
}
```

接下来是确定数据接口与算法，自然就是使用 `map`和 `for` 遍历下就可以求出众数了

```go
m := make(map[int]int, len(nums))

for _, num := range nums {
    if m[num]++; m[num] > len(nums) / 2 {
        return num
    }
}

return -1
```

但是用 `map` 来解决的方法不是最优的，最优的应该是使用 **Boyer-Moore 投票算法**，简单解释如下

我们假设众数为1，其他数为-1，那么所有数加起来必然是大于1的。我们用 `candidate` 存储候选众数，`count` 存储候选众数出现的次数

```go
candidate := 0
count := 0

for _, num := range nums {
    // 如果 count 为0，则表示当前 candidate 为众数的可能性降低，替换为当前的 num 作为新的 candidate
    if count == 0 {
        candidate = num
    }

    if candidate == num {
        count++
    } else {
        count--
    }
}

// 记得我们最前面的假设了吗？假设众数为1，其他数为-1，那么所有数加起来必然是大于1的。所以遍历完成后的候选众数必然是众数
return candidate
```

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/array/majority_element.go)
