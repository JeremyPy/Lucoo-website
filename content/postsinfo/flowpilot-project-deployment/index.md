---
title: "FlowPilot 项目地址与部署准备：CPA / Sub2API 怎么选"
date: 2026-05-20T10:00:32+08:00
lastmod: 2026-05-20T10:00:32+08:00
description: "整理 CPA 和 Sub2API 的项目地址、部署前提和常见坑，适合准备自建自动化链路前先核对环境。"
slug: "flowpilot-cpa-sub2api-deployment"
tags: ["FlowPilot", "ChatGPT", "部署", "自动化"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

> 本文是 Lucoo 根据 FlowPilot 使用文档重新整理的实操版。需要现成账号、Plus 相关卡密、API 额度或不想自己排查环境，可以先看店铺：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)。
>
> 常用入口：中转站 [https://api.lucoo.net](https://api.lucoo.net)，转长链工具 [https://gpt.mooizz.com/](https://gpt.mooizz.com/)，配套工具 [https://faka.gsyun.cloud/](https://faka.gsyun.cloud/)，Plus 跳手机 / JSON 工具 [https://gtxx3600.github.io/GPTSession2CPAandSub2API/](https://gtxx3600.github.io/GPTSession2CPAandSub2API/)。

## 先看结论

- `来源标识`: `project-and-deployment`
- `主题`: `cpa`、`sub2api`、项目地址、部署前提、部署环境
- 本文会按实际操作顺序补充原因、检查点和常见处理方式。

## 适用场景

- 需要拉取并部署 `cpa` 项目
- 需要拉取并部署 `sub2api` 项目
- 需要先确认项目地址和部署前提

## 准备内容

- 可以访问 `GitHub`
- 一个可用的本地目录或服务器目录
- 基础的 `git` 使用能力，或者可以手动下载压缩包
- 如果要部署 `cpa`，部署环境必须可以访问 `OpenAI`

## 操作步骤

### 第一步：确认项目地址

- `cpa` 项目地址：`https://github.com/router-for-me/CLIProxyAPI`
- `sub2api` 项目地址：`https://github.com/Wei-Shaw/sub2api`

先确认你本次要部署的是哪个项目，再决定后续拉取和配置范围。

### 第二步：拉取项目到本地

可以用 `git clone` 拉取，也可以直接下载压缩包后解压。
如果你准备后续持续更新，建议优先使用 `git clone`。

### 第三步：确认部署方式

本教程只负责把项目地址和部署前提说明清楚，不在这里展开完整部署命令。
项目拉取到本地后，可以再让 AI 结合当前环境继续完成部署。

### 第四步：确认 `cpa` 的环境要求

如果你部署的是 `cpa`，部署所在环境必须可以访问 `OpenAI`。
如果环境本身访问不到 `OpenAI`，后续认证流程可能会异常。

## 常见问题

### 为什么部署前要先分清 `cpa` 和 `sub2api`？

因为两者仓库不同、运行方式不同、后续配置项也不同。
如果一开始就搞混，后面让 AI 继续部署时很容易走错方向。

### 为什么认证看起来成功了，但没有生成认证文件？

常见原因是 `cpa` 部署环境无法访问 `OpenAI`。
这种情况下，前面看起来像是执行完成了，但关键结果并没有真正落盘。

## 注意事项

- 先确认本次目标项目，再继续部署
- 需要长期维护时，尽量用 `git` 拉取，而不是每次手动覆盖
- 部署 `cpa` 前，先确认环境确实能访问 `OpenAI`

## Lucoo 补充说明

`cpa` 和 `sub2api` 不是同一个东西，建议先想清楚你的目标：如果你要把 ChatGPT 登录态转成可调用的接口，重点看 `cpa`；如果你要把订阅或账号池整理成统一 API，才继续看 `sub2api`。前期不要同时开太多变量，否则部署失败时很难判断是网络、认证还是项目配置问题。

如果只是想稳定使用模型，不一定非要自建。可以直接使用 Lucoo 中转站 [https://api.lucoo.net](https://api.lucoo.net)，额度从店铺购买：[https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo)，充值后生成 API Key 接入 Cherry Studio、Codex、CodeBuddy 等客户端即可。

## 排查顺序

1. 先确认服务器能访问 GitHub 和 OpenAI。
2. 再确认 Node、Python、Docker 等运行环境是否满足项目要求。
3. 最后再看项目配置、环境变量和认证文件。

这样排查的原因很简单：网络不通时，后面所有命令看起来都像配置错误，其实只是请求没有真正到达目标服务。
