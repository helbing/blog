+++
title = "Valid Parentheses"
date = 2020-09-02T08:49:11+08:00
tags = ["leetcode"]
draft = false
+++

<!--more-->

## 题目

Given a string s containing just the characters `(`, `)`, `{`, `}`, `[` and `]`, determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.

Example 1:

```text
Input: s = "()"
Output: true
```

Example 2:

```text
Input: s = "()[]{}"
Output: true
```

Example 3:

```text
Input: s = "(]"
Output: false
```

Example 4:

```text
Input: s = "([)]"
Output: false
```

Example 5:

```text
Input: s = "{[]}"
Output: true
```

Constraints:

- `1 <= s.length <= 10^4`
- `s` consists of parentheses only `()[]{}`.

> 题目地址: [Valid Parentheses](https://leetcode-cn.com/problems/valid-parentheses/)

## 解题思路

首先确定边界问题

```go
if len(s) == 0 {
    return true
}

if len(s) == 1 {
    return false
}
```

这道题看起来使用的数据结构就是栈

```go

type Stack struct {
    stack  []byte
    length int
}

func (s *Stack) Push(b byte) {
    s.stack = append(s.stack, b)
    s.length++
}

func (s *Stack) Pop() (res byte) {
    if s.length <= 0 {
        return
    }

    s.length--
    res = s.stack[s.length]
    s.stack = s.stack[0:s.length]
    return
}

func (s *Stack) IsEmpty() bool {
    return s.length <= 0
}

func NewStack() *Stack {
    return &Stack{}
}
```

算法也比较简单，遍历字符串，如果是`(`, `[`, `{`就入栈，如果是`)`, `]`, `}`，就出栈，然后进行比对

```go
func isValid(s string) bool {
    if len(s) == 0 {
        return true
    }

    if len(s) == 1 {
        return false
    }

    stack := NewStack()

    for i := 0; i < len(s); i++ {
        if s[i] == '(' || s[i] == '[' || s[i] == '{' {
            stack.Push(s[i])
        } else {
            if stack.IsEmpty() {
                return false
            }

            top := stack.Pop()
            if top == '(' && s[i] != ')' {
                return false
            } else if top == '[' && s[i] != ']' {
                return false
            } else if top == '{' && s[i] != '}' {
                return false
            }
        }
    }

    return stack.IsEmpty()
}
```

> 答案地址: [Github](https://github.com/helbing/leetcode/blob/master/stack/valid_parentheses.go)
