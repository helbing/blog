+++
title = "《人月神话》读书笔记"
date = 2020-08-23T16:36:23+08:00
tags = ["reading"]
draft = false
+++

首先用代序中我很认同的一句话作为开头

> 回首软件工程近40年的发展，Jackson 哀叹软件行业普遍缺乏专业性，充满了业余人员，“手中有一个锤子，看到什么都是钉子”，谁都能开发性命攸关的软件

<!--more-->

## 焦油坑

作者回顾过去几十年开发过的大型系统，只有极少数的项目能同时满足目标，进度和预算的要求。每个问题看起来都能解决，但是当它们互相纠缠和累积起来的时候就如同一个焦油坑，任何一个团队都会越陷越深，无法看清问题的本质

## 人月神话

作者用餐厅菜单中的一句话来表达这章的主题内容

> 美食的烹调需要时间，片刻等待，更多美味，更多享受

在众多软件项目中，**缺乏合理的进度安排是造成项目滞后的最主要原因**，作者总结了一些造成这种灾难的普遍原因，但是在我看来其中最重要的是对于人月关系，即人力(Man)与时间(Month)的理解。作者指出人力与时间不是纯粹的线性关系的，比如2个人花了2个月完成了项目，那么4个人花1个月同样也能完成项目。这种观点是错误的，它忽略了问题的复杂度，培训成本与沟通成本等。对此作者提出了 Brooks 原则

> 向进度落后的项目中增加人手，只会使进度更加落后

对于如何制定合理的项目进度，作者分享了自己多年的经验法则

- 1/3计划。我的理解是任务拆解，系统设计，排期等
- 1/6编码
- 1/4构建测试和早期系统测试。我的理解是单元测试与系统自测
- 1/4系统测试，所有的构建已完成。我的理解是冒烟测试，集成测试，功能测试等

## 外科手术团队

作者指出了大型的项目团队中，最好的与最差的程序员表现出来的生产率与质量具有明显的差异，所以作者更加推崇外科手术团队式的精简团队，将项目拆解，每一个部分由一个团队解决

## 贵族专制，民主政治和系统设计

作者指出概念的完整性要求设计(这里我认为是指系统设计)必须由一个人，或者非常少数且互有默契的人员来实现

## 画蛇添足

看标题可能会摸不着头脑，但是如果看到英文标题，The Second-System Effect，可能会稍微有点感觉

有个问题是我们在项目开发中时刻面对着的，那就是成本超出预算。这时候能采用的应对方式无非两种

1. 砍掉不重要的需求与设计
2. 采用成本更低的实现方式

如果采用第二种方法，你需要明确的是

1. 开发人员来实现，你只能建议，不能支配
2. 自己的建议不是绝对的，可能开发人员会提出更好的方法
3. 私底下进行建议
4. 时刻准备放弃自己的建议

还有一个问题就是过度设计的问题，当项目开发完成后，下一轮开发时，架构师往往会提出一些优化建议，可能最终会开发出一些用不上的功能，这也是为什么标题会叫画蛇添足。那么如何避免画蛇添足，如下

1. 至少经过拥有两个系统以上开发经验的架构师的决定
2. 警惕诱惑

## 贯彻执行

作者这章主要提规范，感觉这章重要的东西不多

1. 文档手册与规格说明要规范统一，每次修改必须指明修改人与修改时间
2. 会议
3. 电话日志

## 为什么巴比伦塔会失败

距《创世纪》记载，巴比伦塔是人类继诺亚方舟之后的第二大工程壮举，同时，其也是第一个彻底失败的工程。作者指出建造巴比伦塔时不缺

1. 清晰的目标
2. 人力
3. 材料
4. 足够的时间
5. 足够的技术

但是最终巴比伦塔失败的主要原因是交流与组织。交流主要的方式有

1. 非正式途径
2. 会议
3. 规范化手册

组织，作者指出三种组织架构

1. 产品负责人与技术主管是同一个人
2. 产品负责人作为总指挥，技术主管充当其左右手
3. 技术主管作为总指挥，产品负责人充当其左右手

## 胸有成竹

这章讲了下如何估算工作量

```text
工作量 = 常数 * 指令数量^1.5
```

- 指令数量，考虑到当时基本是使用汇编开发，指的应该是汇编指令
- 1.5这个数字是研究得出的结果

## 削足适履

这章主要讲编程时面临的问题，空间换时间与时间换空间的问题，算法与数据结构的重要性

## 提纲挈领

这章再次强调文档的重要性

## 未雨绸缪

1. 唯一不变的就是变化本身
2. 为变更设计系统

## 干将莫邪

这章强调了工具的重要性

## 整体部分

1. 自上而下的设计与结构化编程
2. 单元测试
3. 集成测试

## 祸起萧墙

> 项目怎么会被延迟了整整一年的时间
> 延迟的时间是一天天积累下来的

## 另外一面

1. 文档，比如需求文档，接口文档，测试用例文档等
2. 流程图，比如 UML 图，ER 图等。虽然作者很不喜欢流程图，但是就我个人而已，我还是蛮喜欢的，特别是在做系统设计时，或者是在给别人讲解设计理念时，确实能起到很大的帮助
3. 自文档化，比如说，好的注释，好的命名，好的编程规范等，总之好的代码具备**自解释能力**

## 没有银弹

所有软件活动包括：根本任务，即打造构成抽象软件实体店复杂概念结构。次要任务，即使用编程语言表达这些抽象实体，在空间和时间限制下将它们映射为机器语言

现代软件开发系统中无法规避的内在特性

1. 复杂性
2. 一致性
3. 可变性
4. 不可见性

作者还提到一些潜在的银弹

1. 高级编程语言，比如 C/C++，Java 等
2. 面向对象
3. 增量式开发

## 再论“没有银弹”

作者认为软件开发中的根本任务是应该优先进行处理的，其次才是次要任务。这点我是非常认同的，这些年见过太多的程序员接到需求后就开始干活了，没想过如何设计，单纯为了实现功能而写代码。同时也见到了太多的公司将软件开发简单的理解为只是写代码而已，留给程序员思考设计的时间可以说基本没有

## 《人月神话》的观点：是与非

这章相当于总结，建议看原书

## 20年后的《人月神话》

这章同样建议看原书