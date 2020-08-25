+++
title = "Best Time to Buy and Sell Stock"
date = 2020-08-25T12:59:08+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Say you have an array for which the `i^th` element is the price of a given stock on day i.

If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.

Note that you cannot sell a stock before you buy one.

Example 1:

```text
Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5. Not 7-1 = 6, as selling price needs to be larger than buying price.
```

Example 2:

```text
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```

> 题目地址: [Best Time to Buy and Sell Stock](https://leetcode-cn.com/problems/best-time-to-buy-and-sell-stock/)

## 解题思路

首先确定下边界

```go
if len(prices) <= 1 {
    return 0
}
```

接下来确定使用的数据结构，看样子用数组即可。最后确定下算法，考虑到 `prices`的大小没有限制，应该考虑使用 DP，`dp(i)` 表示第 i 天卖出时的最大收益

```text
dp(0) = 0
dp(1) = max(0, dp(0) + prices(1) - prices(0))
dp(2) = max(0, dp(1) + prices(2) - prices(1))
...
dp(i) = max(0, dp(i) + prices(i) - prices(i-1))
```

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/array/best_time_to_buy_and_sell_stock.go)
