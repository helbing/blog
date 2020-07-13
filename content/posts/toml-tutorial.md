+++
title = "TOML 入门教程"
date = 2020-07-13T00:00:00+08:00
lastmod = 2020-07-13T13:24:23+08:00
tags = ["toml"]
draft = true
+++

[TOML](https://github.com/toml-lang/toml) 的全称是 Tom's Obvious Minimal Language，因为它的作者是 GitHub 联合创始人 Tom Preston-Werner。TOML 的目的是成为一个极简的配置文件格式，可以无歧义地被映射为 hasmap，从而被多种语言解析

<!--more-->


## 格式约定 {#格式约定}

-   TOML 是大小写敏感的
-   编码格式使用 UTF-8
-   使用 `#` 进行注释
-   boolean 类型使用 true/false，小写格式
-   时间日期遵循 [rfc3339](https://tools.ietf.org/html/rfc3339)


## 语法 {#语法}


### string {#string}


#### single {#single}

```toml
key = "value"
```


#### multi {#multi}

```toml
key = """
value1
value2
"""
```

等同于

```toml
key = "value1\r\nvalue2\r\nvalue3"
```

如果不需要换行，则

```toml
key = """
value1 \
value2
"""
```

等同于

```toml
key = "value1 value2"
```


### array {#array}

```toml
key = [
  "value1"
  "value2"
]
```

`记住，每个数组元素的数据类型必须一致`


### hashmap {#hashmap}

```toml
[table]
key = "value"
```

等同于

```json
{"table":{"key":"value"}}
```

hashmap 还能嵌套 hashmap

```toml
[first.second]
key = "value"
```

等同于

```json
{"first":{"second":{"key":"value"}}}
```


### hashmap array {#hashmap-array}

```toml
[[demo]]
key = "value"

[[demo]]
key = "value"
```

等同于

```json
[{"key":"value"},{"key":"value"}]
```


## 为什么需要 TOML {#为什么需要-toml}

关于这点可以直接看作者的回答，[Comparison with Other Formats](https://github.com/toml-lang/toml#comparison-with-other-formats)
