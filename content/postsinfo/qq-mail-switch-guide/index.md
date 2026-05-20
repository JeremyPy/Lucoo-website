---
title: "QQ 邮箱切换教程：英文邮箱和 Foxmail 删除后重建"
date: 2026-05-20T10:00:32+08:00
lastmod: 2026-05-20T10:00:32+08:00
description: "讲清楚 QQ 邮箱、英文邮箱和 Foxmail 地址的切换方式，适合需要快速更换邮箱地址的场景。"
slug: "qq-mail-foxmail-switch"
tags: ["QQ邮箱", "Foxmail", "邮箱", "FlowPilot"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

> 本文是 Lucoo 根据 FlowPilot 使用文档重新整理的实操版。需要现成账号、Plus 相关卡密、API 额度或不想自己排查环境，可以先看店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。
>
> 常用入口：中转站 [https://cc.lucoo.net](https://cc.lucoo.net)，转长链工具 [https://gpt.mooizz.com/](https://gpt.mooizz.com/)，配套工具 [https://faka.gsyun.cloud/](https://faka.gsyun.cloud/)，Plus 跳手机 / JSON 工具 [https://gtxx3600.github.io/GPTSession2CPAandSub2API/](https://gtxx3600.github.io/GPTSession2CPAandSub2API/)。

## 先看结论

- `来源标识`: `qq-mail-switch`
- `主题`: `QQ 邮箱`、英文邮箱、`Foxmail`、删除后重建
- 本文会按实际操作顺序补充原因、检查点和常见处理方式。

## 适用场景

- 当前 `QQ 邮箱` 地址需要切换
- 想继续复用同一个 `QQ 邮箱` 主账号
- 需要创建和删除英文邮箱、`Foxmail` 邮箱

## 准备内容

- 一个可以正常登录的 `QQ 邮箱`
- 可以访问 `账号与安全` 页面

## 操作步骤

### 第一步：登录 `QQ 邮箱`

先登录你当前正在使用的 `QQ 邮箱`。

### 第二步：进入 `账号与安全`

打开：

- `https://wx.mail.qq.com/account/index?sid=zdd4Voy7S04uZjBnAKhFZQAA#/`

### 第三步：进入 `账号管理`

在 `账号与安全` 页面中找到 `账号管理` 入口。

### 第四步：创建英文邮箱和 `Foxmail` 邮箱

在 `账号管理` 中：

1. 创建一个英文邮箱地址
2. 再创建一个 `Foxmail` 邮箱地址

### 第五步：使用后删除并重新创建

这两个地址使用完后，可以直接删除。
删除后重新创建新的英文邮箱和 `Foxmail` 邮箱，就可以继续使用。

## 常见问题

### 这些地址删除后还能继续重复创建吗？

可以。
按原流程删除后，再创建新的英文邮箱和 `Foxmail` 邮箱即可继续使用。

## 注意事项

- 使用完成后尽量及时删除，避免混淆当前正在使用的地址
- 建议每次切换后都确认插件里使用的是最新邮箱地址

## Lucoo 补充说明

QQ 邮箱这套方法的价值在于“主账号不变，邮箱别名可换”。如果你只是需要临时替换注册邮箱，英文邮箱和 Foxmail 删除后重建会比较快；如果你需要长期批量自动化，建议使用域名邮箱或 Cloudflare Temp Email，后期更容易排查。

每次切换完邮箱后，都要回插件确认当前读取的是新地址，不要只在 QQ 邮箱后台改完就继续跑流程。自动化工具通常不会猜你的邮箱变了，它只会使用当前配置里保存的地址。

如果流程卡在账号、Plus 或 API 额度上，可以看店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。
