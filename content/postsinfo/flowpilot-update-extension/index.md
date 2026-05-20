---
title: "FlowPilot 扩展怎么更新：git pull、GitHub Desktop 与手动覆盖"
date: 2026-05-20T10:00:32+08:00
lastmod: 2026-05-20T10:00:32+08:00
description: "把扩展更新、浏览器重新加载和本地改动备份讲清楚，避免文件更新了但浏览器还在跑旧版本。"
slug: "flowpilot-update-extension"
tags: ["FlowPilot", "浏览器扩展", "教程"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
---

> 本文是 Lucoo 根据 FlowPilot 使用文档重新整理的实操版。需要现成账号、Plus 相关卡密、API 额度或不想自己排查环境，可以先看店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。
>
> 常用入口：中转站 [https://cc.lucoo.net](https://cc.lucoo.net)，转长链工具 [https://gpt.mooizz.com/](https://gpt.mooizz.com/)，配套工具 [https://faka.gsyun.cloud/](https://faka.gsyun.cloud/)，Plus 跳手机 / JSON 工具 [https://gtxx3600.github.io/GPTSession2CPAandSub2API/](https://gtxx3600.github.io/GPTSession2CPAandSub2API/)。

## 先看结论

- `来源标识`: `update-extension`
- `主题`: `git pull`、`GitHub Desktop`、手动下载覆盖、扩展重新加载
- 本文会按实际操作顺序补充原因、检查点和常见处理方式。

## 适用场景

- 已经安装过扩展，想更新到最新版本
- 想知道哪种更新方式更省事
- 更新完成后不知道浏览器里还要不要再操作一次

## 准备内容

- 当前扩展的本地文件夹
- 可以打开浏览器的 `扩展程序管理` 页面
- 至少具备下面 3 种更新方式中的一种：
  - `GitHub Desktop`
  - `git`
  - 手动下载最新压缩包

## 操作步骤

### 第一步：选择更新方式

推荐优先级如下：

1. `git pull`
2. `GitHub Desktop`
3. 手动下载覆盖

### 第二步：使用 `GitHub Desktop` 更新

如果你已经把仓库交给 `GitHub Desktop` 管理，后续只需要在软件里拉取最新提交即可。
这种方式适合不想直接敲命令的人。

### 第三步：使用 `git pull` 更新

这是最方便、最推荐的方式。

1. 先安装 `git`
2. 打开终端
3. 执行 `cd 扩展文件夹路径`
4. 再执行 `git pull`

### 第四步：手动下载覆盖更新

如果你不用 `git`，也可以直接下载最新版本到本地，然后覆盖现有扩展文件夹。
这种方式适合临时更新，但不适合频繁维护。

### 第五步：重新加载扩展

不论你用了哪种更新方式，更新完成后都必须：

1. 打开浏览器的 `扩展程序管理`
2. 找到当前扩展
3. 手动点击一次 `重新加载`

## 常见问题

### 为什么我已经更新文件了，但浏览器里还是旧版本？

因为浏览器不会自动重新读取本地扩展目录。
如果你没有点击 `重新加载`，浏览器可能仍在继续使用旧版本。

### 哪种方式最推荐？

推荐使用 `git pull`。
它最省事，也最适合长期更新。

## 注意事项

- 更新完成后一定要手动 `重新加载` 扩展
- 频繁维护时，尽量不要只靠手动下载覆盖
- 如果本地改动较多，执行 `git pull` 前先确认是否需要备份本地修改

## Lucoo 补充说明

扩展更新后一定要点一次浏览器里的 `重新加载`。很多人以为文件覆盖就完事了，结果浏览器仍然缓存旧脚本，自动化流程继续按旧逻辑跑，后面支付、邮箱、代理相关步骤就容易出现莫名其妙的问题。

如果你是按 Lucoo 店铺教程操作，更新前建议先截图保存当前配置，尤其是 API Key、Temp Email、GPC、短信 Helper 这些字段。需要配套卡密或工具入口可以看：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo) 和 [https://faka.gsyun.cloud/](https://faka.gsyun.cloud/)。

## 推荐更新策略

长期使用就用 `git pull`，临时尝鲜才手动覆盖。手动覆盖最大的问题是容易把自己的配置文件一起盖掉，所以覆盖前先确认哪些文件是程序文件，哪些文件是你自己的本地配置。
