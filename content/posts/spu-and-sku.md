+++
title = "商品 SPU 和 SKU 设计思考"
date = 2020-08-13T21:40:36+08:00
tags = ["system_design"]
draft = false
+++

最近由于公司业务调整需要开发一个电商系统，也就不得不涉及到 SPU 和 SKU 的设计。首先介绍下 SPU 和 SKU 的概念

- SPU: Standard Product Unit，标准产品单位，指代的是一类商品的共同属性
- SKU: Stock Keeping Unit，库存量单位，指代的是商品属性的库存量

<!--more-->

SPU 相比于 SKU 更好理解，也更好实现

![](/blog/774919BB-CF63-4B45-AA92-3BA6A8C83BD0.png)

我们可以看到 3C 份类下的手机类产品都是有共同的商品属性的，比如

- 品牌
- 产品参数
  - 产品名称
  - 3C产品型号
  - 型号
  - 网络模式
  - 等等

![](/blog/E13E3CCE-EFD5-42F4-8214-60179BCA24C7.png)

商品的 SPU 看起来还比较容易设计的。接下来我们来看 SKU

![](/blog/A2578FC5-E7CD-42B0-A750-961F2C344977.png)

可以看到我们选择不同的商品 SKU，得到的库存数量和价格是不一样的。为了方便理解 SKU，我们来看看后台页面是如何设置商品 SKU 的

![](/blog/770652EE-0C2E-43E4-8873-CA9193507E28.png)

有很多参数，但是我们着重看商品的多规格，价格和库存的设置即可。其实还可以理解为 SKU 是一种特殊的 SPU

![](/blog/B71CD667-6007-457C-8EE2-FF146FF3D96B.png)

基于此我们给到前端的 SKU 接口大致如下

```json
{
    "id": 1,
    "name": "iPhone X"
    "sku": [
        {
            "id": 1,
            "price": 5000,
            "stock": 10,
            "sku_combine": [
                {
                    "attr_id": 1,
                    "attr_name": "颜色",
                    "value_id": 1,
                    "value_name": "红色"
                },
                {
                    "attr_id": 2,
                    "attr_name": "存储容量",
                    "value_id": 3,
                    "value_name": "4+64G"
                }
            ]
        }
    ]
}
```

电商系统相比于其他业务系统来说是比较复杂的，有不少设计业务上的设计难点，值得认真琢磨，SPU 和 SKU 就是一个。其他的比方说，购物车，优惠券，订单等，如果是 SaaS 平台，还需要考虑多租户。通过这篇文章，我们大概了解了商品 SPU 和 SKU 的设计，虽然不保证我这个设计是最优的，但是应该算是个还不错的设计，希望能给读者在设计商品 SPU 和 SKU 时带来一些帮助
