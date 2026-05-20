---
title: "Cloudflare Temp Email 配置教程：邮箱生成、转发与随机子域"
date: 2026-05-20T10:00:32+08:00
lastmod: 2026-05-20T10:00:32+08:00
description: "详解 Cloudflare Temp Email 在自动化流程里的 Temp API、Admin Auth、Custom Auth、随机子域和收件邮箱配置。"
slug: "cloudflare-temp-email-flowpilot"
tags: ["Cloudflare", "临时邮箱", "FlowPilot", "邮箱"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
---

> 本文是 Lucoo 根据 FlowPilot 使用文档重新整理的实操版。需要现成账号、Plus 相关卡密、API 额度或不想自己排查环境，可以先看店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。
>
> 常用入口：中转站 [https://cc.lucoo.net](https://cc.lucoo.net)，转长链工具 [https://gpt.mooizz.com/](https://gpt.mooizz.com/)，配套工具 [https://faka.gsyun.cloud/](https://faka.gsyun.cloud/)，Plus 跳手机 / JSON 工具 [https://gtxx3600.github.io/GPTSession2CPAandSub2API/](https://gtxx3600.github.io/GPTSession2CPAandSub2API/)。

## 先看结论

- `来源标识`: `cloudflare-temp-email`
- `主题`: `Cloudflare Temp Email`、`Admin Auth`、`Custom Auth`、随机子域、邮件接收
- 本文会按实际操作顺序补充原因、检查点和常见处理方式。

## 适用场景

- 需要把 `Cloudflare Temp Email` 用作 `邮箱生成`
- 需要把 `Cloudflare Temp Email` 用作 `邮箱服务`
- 需要同时配置 `Temp API`、认证信息、域名和收件邮箱

## 准备内容

- 一个可用的 `Cloudflare Temp Email` 后端地址
- 如果要使用随机子域，对应域名解析已经提前配置好
- 后端的 `admin auth`
- 如果站点额外设置了访问密码，对应的访问认证信息
- 一个真正用于接收转发邮件的收件邮箱

## 操作步骤

### 第一步：先确认当前用途

`Cloudflare Temp Email` 可以同时承担两类角色：

- `邮箱生成`
- `邮箱服务`

如果两边都选择了它，就需要把两套配置都填完整。

### 第二步：填写 `Temp API`

在插件中先填写 `Temp API`，例如：

- `https://your-worker-domain`

不论你把它用于 `邮箱生成` 还是 `邮箱服务`，这一项都必须先配好。

### 第三步：按需填写 `Admin Auth`

如果你把 `邮箱生成` 选择成 `Cloudflare Temp Email`，就需要填写 `Admin Auth`。
它对应后端配置里的 `admin auth`。

### 第四步：按需填写 `Custom Auth`

`Custom Auth` 只有在站点额外开启访问密码时才需要填写。
如果没有这层额外访问密码，留空即可。
它不会替代 `Admin Auth`。

### 第五步：配置 `Temp 域名`

这里填写允许创建邮箱的基础域名。
即使你开启了 `随机子域`，这里仍然填写基础域名，而不是随机出来的子域名。

### 第六步：按需开启 `随机子域`

只有在 `邮箱生成 = Cloudflare Temp Email` 时，这一项才会生效。
启用前需要先确认：

- 后端已经配置 `RANDOM_SUBDOMAIN_DOMAINS`
- Cloudflare DNS 已经设置 `MX *`

### 第七步：作为 `邮箱服务` 时填写 `邮件接收`

如果 `邮箱服务` 也选了 `Cloudflare Temp Email`，还需要填写真正的收件邮箱。
后续转发邮件会送到这里。

### 第八步：查看后端搭建参考

如果你还没有部署后端，可以参考：

- `https://linux.do/t/topic/316819`

## 常见问题

### 为什么我明明配了 `Temp API`，还是不能生成邮箱？

先确认：

- `邮箱生成` 是否真的选了 `Cloudflare Temp Email`
- `Admin Auth` 是否正确
- `Temp 域名` 是否正确

### 为什么随机子域没有生效？

通常是因为后端没有配置 `RANDOM_SUBDOMAIN_DOMAINS`，或者 Cloudflare DNS 没有完成 `MX *` 设置。

## 注意事项

- 如果同时把它用作 `邮箱生成` 和 `邮箱服务`，要把两边相关字段都检查一遍
- `Custom Auth` 只有额外访问密码场景才需要填写
- 开启随机子域前，先确认后端和 DNS 已经准备好

## Lucoo 补充说明

临时邮箱的核心不是“能生成一个地址”，而是“OpenAI 发来的验证邮件能稳定送到你能读取的地方”。所以配置时要同时检查生成端、转发端和收件端。少配任何一层，前台看起来都可能正常，但验证码就是收不到。

如果你只是为了买现成 ChatGPT 账号或 Plus 相关服务，可以走店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。如果你想把自己的账号池接到中转站，建议注册 [https://cc.lucoo.net](https://cc.lucoo.net) 后用 API Key 管理调用，不用每次都重新折腾邮箱链路。

## 检查清单

1. `Temp API` 能在浏览器正常打开。
2. `Admin Auth` 和后端配置完全一致。
3. 域名 MX 解析已经生效。
4. 转发邮箱能收到普通测试邮件。
5. 插件里选择的“邮箱生成”和“邮箱服务”与实际配置一致。
