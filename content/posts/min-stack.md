+++
title = "Min Stack"
date = 2020-09-03T08:25:39+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

- push(x) -- Push element x onto stack.
- pop() -- Removes the element on top of the stack.
- top() -- Get the top element.
- getMin() -- Retrieve the minimum element in the stack.

Example 1:

```text
Input
["MinStack","push","push","push","getMin","pop","top","getMin"]
[[],[-2],[0],[-3],[],[],[],[]]

Output
[null,null,null,null,-3,null,0,-2]

Explanation
MinStack minStack = new MinStack();
minStack.push(-2);
minStack.push(0);
minStack.push(-3);
minStack.getMin(); // return -3
minStack.pop();
minStack.top();    // return 0
minStack.getMin(); // return -2
```

Constraints:

- Methods pop, top and getMin operations will always be called on `non-empty`stacks.

> 题目地址: [Min Stack](https://leetcode-cn.com/problems/min-stack/)

## 解题思路

这道题解题方式就是用两个数据栈，一个用于存储全部值，一个用于存储最小值

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/stack/min_stack.go)
