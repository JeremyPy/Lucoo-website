---
title: "iCloud 隐私邮箱批量使用教程：隐藏邮件地址与插件读取"
date: 2026-05-20T10:00:32+08:00
lastmod: 2026-05-20T10:00:32+08:00
description: "说明 iCloud+ 隐藏邮件地址如何准备、转发、刷新到插件里，以及为什么要控制创建频率。"
slug: "icloud-hide-my-email-flowpilot"
tags: ["iCloud", "隐私邮箱", "FlowPilot", "ChatGPT"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

> 本文是 Lucoo 根据 FlowPilot 使用文档重新整理的实操版。需要现成账号、Plus 相关卡密、API 额度或不想自己排查环境，可以先看店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。
>
> 常用入口：中转站 [https://api.lucoo.net](https://api.lucoo.net)，转长链工具 [https://gpt.mooizz.com/](https://gpt.mooizz.com/)，配套工具 [https://faka.gsyun.cloud/](https://faka.gsyun.cloud/)，Plus 跳手机 / JSON 工具 [https://gtxx3600.github.io/GPTSession2CPAandSub2API/](https://gtxx3600.github.io/GPTSession2CPAandSub2API/)。

## 先看结论

- `来源标识`: `icloud-private-email`
- `主题`: `iCloud+`、`隐藏邮件地址`、Apple ID、转发邮箱、插件刷新
- 本文会按实际操作顺序补充原因、检查点和常见处理方式。

## 适用场景

- 需要把 `iCloud+` 的 `隐藏邮件地址` 用作隐私邮箱
- 需要先在 Apple 设备上开通 `iCloud 邮件`
- 需要在插件中读取已经创建好的隐私邮箱

## 准备内容

- 一个已开通 `iCloud+` 的 Apple ID
- 一台可登录该 Apple ID 的 `iPhone`、`iPad` 或 `Mac`
- 一个真正用于接收转发邮件的邮箱
- 插件中可正常使用的邮箱服务配置

## 操作步骤

### 第一步：准备 Apple ID

如果条件允许，建议使用非日常主力 Apple ID。
这样即使后续触发风控，也不会影响你平时常用的账号。

### 第二步：在 `iPhone` 或 `iPad` 上启用 `iCloud 邮件`

1. 打开 `设置`
2. 点击顶部你的姓名
3. 进入 `iCloud`
4. 找到 `邮件`
5. 打开右侧开关

如果是首次开通，按提示创建一个 `@icloud.com` 主邮箱地址。

### 第三步：在 `Mac` 上确认 `iCloud 邮件` 已开启

1. 打开苹果菜单
2. 进入 `系统设置`
3. 点击你的姓名
4. 进入 `iCloud`
5. 确认 `iCloud 邮件` 已启用

### 第四步：进入 `隐藏邮件地址`

在 `iCloud` 页面往下找到 `iCloud+`，然后进入 `隐藏邮件地址`。

### 第五步：先手动创建一批隐私邮箱

建议先手动创建一批，例如 `20` 个左右。
这样插件后续读取时更稳定，也更容易判断整个流程有没有打通。

同时确认 `转发至` 已经设置为你真正接收邮件的邮箱。

### 第六步：配置插件中的邮箱服务

在插件里把 `邮箱服务` 设置为你真正接收转发邮件的邮箱，并确认它能正常收信。

### 第七步：配置插件中的邮箱生成

1. 将 `邮件生成` 选择为 `iCloud 隐私邮箱`
2. 先在网页中登录你的 `iCloud`
3. 回到插件的隐私邮箱配置区域
4. 点击刷新
5. 等待插件读取你已经创建好的地址

### 第八步：选择邮箱使用方式

建议优先使用 `复用未使用的`。
如果你希望释放数量，可以勾选 `使用后删除`。
如果不删除，这些地址可以保留为长期邮箱。

### 第九步：控制创建频率

建议每天最多新增 `3` 个左右。
测试中一天创建过多容易触发 `iCloud` 风控。

### 第十步：查看官方说明

- `Create a primary email address for iCloud Mail`
- `Create and edit Hide My Email addresses on iCloud.com`

## 常见问题

### 插件刷新后没有看到任何邮箱怎么办？

先确认：

- 你已经在网页里登录了 `iCloud`
- 你已经手动创建过隐私邮箱
- `转发至` 已经设置好

确认后，再回插件里刷新一次。

### 要不要勾选 `使用后删除`？

如果你想及时释放可用数量，可以勾选。
如果你想把地址当长期邮箱保留，可以不勾选。

### 为什么不建议大量新建？

因为短时间创建过多容易触发 `iCloud` 风控，后面会影响正常使用。

## 注意事项

- 优先使用非日常主力 Apple ID
- 建议先手动创建一批地址，再让插件读取
- 一天不要大量新建，建议控制在 `3` 个左右
- 如果长期不删除，数量可能会较快达到上限

## Lucoo 补充说明

iCloud 隐私邮箱适合低频、干净、可回收的注册场景。它的优点是转发稳定，缺点是创建太猛容易触发 Apple 风控，所以不要把它当成无限邮箱池使用。

实际操作里，建议先手动创建一批，再让插件读取。这样做的原因是：手动创建能先验证 Apple ID、iCloud 邮件、转发邮箱是否都正常；插件读取只是后续自动化，不应该承担第一次打通链路的风险。

不想自己维护邮箱和代理的小伙伴，可以直接购买现成服务：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。后续使用模型时也可以走 Lucoo 中转站：[https://api.lucoo.net](https://api.lucoo.net)。
