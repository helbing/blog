+++
title = "关系型数据库规范"
date = 2020-09-07T06:52:16+08:00
tags = ["mysql"]
draft = false
+++

这是一套我正在使用的规范，所有的规范遵守

- 必须 (MUST)，严格遵循
- 一定不 (MUST NOT)，严格禁止
- 应该 (SHOULD)，建议，不强求
- 不应该 (SHOULD NOT) 不建议，不强求
- 等等，其他的可以看 [rfc2119](https://www.ietf.org/rfc/rfc2119.txt)

<!--more-->

## 数据库与数据表

- [MUST] 库名与表名必须控制在 32 个字符以内
- [MUST] 库名与表名的命名必须使用小写字母，数组与下划线
- [SHOULD] 库名与表名不使用复数名词，仅仅用于解释含义
- [MUST] 数据库与数据表必须注释且必须及时更新
- [SHOULD] 字符集使用 `utf8mb4`，不然存储不了 emoji，校验规则使用 `utf8mb4-unicode-ci`
- [SHOULD] 每个数据表应该有 `id`，`created_at` 和 `updated_at` 这三个字段
  - `id bigint unsigned not null auto_increment`
  - `created_at timestamp not null default current_timstamp`
  - `updated_at timestamp not null default current_timestamp current_timestamp on update current_timestamp
- [SHOULD] 多对多的关联表命名规则为 `xxx_xxx_relation`，假设有两个表 `user` 和 `book`，那它们的多对多关联表为 `user_book_relation`

## 列字段

- [MUST] 列字段名必须使用小写字母，数组与下划线
- [MUST] 时间类型的列字段的数据类型必须是 `timestamp`，命名方式必须是 `xxxx_at`
- [MUST] 列字段名禁止使用保留字，不止是 SQL 保留字，也包括编程语言的保留字，如 `class`，`select` 等
- [MUST] 列值如果一定为正数必须使用 `unsigned` 关键字约束
- [MUST] 列字段用于表达是否的概念，必须是 `is_xxx` 的命名方式，数据类型必须是 `tinyint unsigned`，0 表示否，1 表示是
- [SHOULD] 列字段如果是用于枚举，应该从 1 开始，这会给开发的时候带来很大的便利
- [MUST] 列字段必须设置默认值，如
  - varchar 和 char 默认值为 null
  - text 默认值为 null
  - timestamp 默认值为 null，除了 `created_at` 和 `updated_at`
  - 等等
- [MUST] 列字段如果长度基本固定，必须使用 `char`
- [MUST] 列字段必须注释且必须及时更新
- [SHOULD] 列字段可以适当冗余，减少关联查询提高查询效率。不必执着于三范式，需要注意的是冗余的字段不应该是
  - 频繁更新的字段
  - 长 varchar 或 text 等
- [SHOULD] 软删除的列字段建议使用 `deleted_at timestamp default null
- [SHOULD] 数据表的列字段数建议少于 20 个，最多不超过 50 个

## 索引

- [MUST] 对于列值一定是唯一的情况，必须使用唯一索引进行约束，如订单号
- [SHOULD] 单表的索引数量不应该超过 5 个，对于 InnoDB 来说，太多的索引会影响 insert 和 update 的效率
- [SHOULD] 索引命名规范
  - 普通索引 `idx_xxx`
  - 唯一索引 `uk_xxx`
  - 主键索引 `pk_xxx`

## 开发规范

- [MUST] 避免数据类型的强制转换
- [MUST] 禁止跨库查询，应当使用不同的数据库账号分开查询
- [SHOULD] 如果预计三年内达不到单表 500 万行或者单表容量达到 2GB，那么在创建表的时候不建议考虑分库分表
- [MUST] 对长 varchar，text 类型等建立索引时，必须使用指定前缀索引，指定索引长度
- [MUST] 禁止模糊查询时使用左模糊和全模糊，如果需要的话使用搜索引擎解决
- [MUST] 设计数据库表与字段时，需要足够抽线，这样便于后续拓展
- [MUST] 对于程序连接数据库的账号，遵循权限最小原则
