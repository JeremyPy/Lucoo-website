---
title: "IDEA / JetBrains Kilo Code 接入 Lucoo 中转站"
date: 2026-06-24T10:05:00+08:00
lastmod: 2026-06-24T10:05:00+08:00
description: "在 IntelliJ IDEA、WebStorm、PyCharm 等 JetBrains IDE 中安装 Kilo Code，并配置 Lucoo OpenAI 兼容 API。"
slug: "kilo-code-lucoo-relay"
tags: ["Kilo Code", "JetBrains", "IDEA", "中转站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

## 一、适用范围

Kilo Code 支持 JetBrains 系列 IDE，例如 IntelliJ IDEA、WebStorm、PyCharm、GoLand 等。它可以使用自己的模型网关，也可以配置自带 API Key 的供应商。

本文按 Lucoo OpenAI 兼容接口来配置。

## 二、安装 Kilo Code 插件

在 JetBrains IDE 中打开：

```text
Settings / Preferences -> Plugins -> Marketplace
```

搜索 `Kilo Code` 并安装。安装完成后重启 IDE。

也可以从 JetBrains Marketplace 打开插件页：

[https://plugins.jetbrains.com/plugin/28350-kilo-code](https://plugins.jetbrains.com/plugin/28350-kilo-code)

## 三、准备 Lucoo API Key

1. 打开 [https://cc.lucoo.net](https://cc.lucoo.net)。
2. 在「钱包」兑换余额。
3. 在「API 密钥」创建 Key。
4. 编程场景建议优先选择 `pro` 分组。
5. 复制 `sk-` 开头的 Key。

不要把兑换码填进 Kilo Code。兑换码只用于后台兑换余额。

## 四、添加 OpenAI 兼容服务商

进入 Kilo Code 设置页，找到模型供应商或 API Provider 设置。

按下面填写：

| 配置项 | 内容 |
| --- | --- |
| Provider 类型 | OpenAI Compatible / Custom OpenAI |
| Provider 名称 | `Lucoo` |
| Base URL | `https://cc.lucoo.net/v1` |
| API Key | Lucoo 后台创建的 `sk-` 开头 Key |
| 模型名 | 后台当前可用模型，例如 `gpt-5.5` |

如果插件界面里没有完全一样的字段名，找含义相同的项填写即可。

## 五、备用入口

如果 IDE 里请求超时或连接失败，可以只切换 Base URL：

- `https://api.lucoo.net/v1`
- `https://hkcc.lucoo.net/v1`
- `https://sgcc.lucoo.net/v1`
- `https://uscc.lucoo.net/v1`

Key 不用变。

## 六、测试方式

打开一个项目，在 Kilo Code 里输入：

```text
请阅读当前项目目录，列出主要文件和可能的启动命令。
```

如果能正常返回，并且没有 401、404 或模型不存在，就说明配置成功。

## 七、常见问题

### 1. 插件里找不到 Kilo Code

确认 JetBrains IDE 版本较新，并且 Marketplace 可以访问。也可以从插件官网页面下载后手动安装。

### 2. API Key 无效

检查 Key 是否是 `sk-` 开头，以及有没有把兑换码当 Key。

### 3. 模型不可用

模型名要和后台支持的模型一致。不同分组可用模型可能不同，建议先用 `pro` 分组。

### 4. 请求地址报错

OpenAI 兼容接口要带 `/v1`，也就是 `https://cc.lucoo.net/v1`。

## 八、参考入口

- Kilo Code 官网：[https://kilo.ai/](https://kilo.ai/)
- Kilo Code JetBrains 文档：[https://kilo.ai/docs/code-with-ai/platforms/jetbrains](https://kilo.ai/docs/code-with-ai/platforms/jetbrains)
- Lucoo 防丢主页：[https://lucoo.net](https://lucoo.net)
