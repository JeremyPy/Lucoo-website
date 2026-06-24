---
title: "Codex CLI 接入 Lucoo 中转站教程"
date: 2026-06-24T10:00:00+08:00
lastmod: 2026-06-24T10:00:00+08:00
description: "Windows、macOS、Linux 和 WSL 用户配置 Codex CLI 使用 Lucoo 中转站 API Key 的完整步骤。"
slug: "codex-cli-lucoo-relay"
tags: ["Codex", "OpenAI", "CLI", "中转站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

## 一、先看清楚发货内容

收到卡密后，一般会看到「使用方法」「兑换码」这类字段。这里最容易弄混：

| 字段 | 应该怎么用 |
| --- | --- |
| 兑换码 | 只在 Lucoo 后台「钱包」里兑换余额，不要填到 Codex 里 |
| API Key | 余额兑换后，在「API 密钥」页面自己创建，Codex 要填的是这个 Key |
| Codex API 地址 | 固定填 `https://cc.lucoo.net/v1`，结尾必须带 `/v1` |
| 备用地址 | 主站不稳定时，把域名换成 `api.lucoo.net`、`hkcc.lucoo.net`、`sgcc.lucoo.net` 或 `uscc.lucoo.net`，路径仍然保留 `/v1` |

<p class="lucoo-token-warning-block">重点：兑换码不是 API Key。先兑换余额，再创建 API Key，最后再配置 Codex。</p>

## 二、登录后台并创建 API Key

1. 打开 [https://cc.lucoo.net](https://cc.lucoo.net)，注册并登录。
2. 进入「钱包」，粘贴兑换码，确认兑换成功。
3. 进入「API 密钥」，点击创建。
4. 分组建议选择 `pro`；轻量使用可以选 `plus`。
5. 保存后复制 `sk-` 开头的 API Key。

如果主站打不开，可以先看防丢主页：[https://lucoo.net](https://lucoo.net)。

## 三、安装 Codex CLI

macOS / Linux 可以优先使用官方安装脚本：

```bash
curl -fsSL https://chatgpt.com/codex/install.sh | sh
```

Windows PowerShell 可以使用：

```powershell
powershell -ExecutionPolicy ByPass -c "irm https://chatgpt.com/codex/install.ps1 | iex"
```

如果你已经装好 Node.js，也可以用 npm：

```bash
npm install -g @openai/codex
```

安装完成后检查版本：

```bash
codex --version
```

## 四、写入 Codex 配置

Codex 的用户级配置目录默认是 `~/.codex`。如果没有这个目录，先创建：

```bash
mkdir -p ~/.codex
```

创建或替换 `~/.codex/auth.json`：

```json
{
  "OPENAI_API_KEY": "sk-这里填你在 Lucoo 后台创建的 API Key"
}
```

创建或替换 `~/.codex/config.toml`：

```toml
model = "gpt-5.5"
model_provider = "lucoo"
model_reasoning_effort = "xhigh"
sandbox_mode = "workspace-write"
approval_policy = "on-request"

[model_providers.lucoo]
name = "Lucoo"
base_url = "https://cc.lucoo.net/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

如果你需要切换备用入口，只改 `base_url`：

```toml
base_url = "https://sgcc.lucoo.net/v1"
```

## 五、启动 Codex

进入你要处理的项目目录，然后执行：

```bash
codex
```

第一次启动时，可以让 Codex 解释当前目录结构，确认它已经可以正常请求模型：

```text
帮我看一下这个项目的目录结构，并总结主要文件作用
```

能正常返回内容，就说明配置已经生效。

## 六、常见问题

### 1. 401 或 invalid api key

通常是 API Key 复制错了、复制了空格，或者把兑换码当成 API Key 填进去了。回到后台重新复制 `sk-` 开头的 Key。

### 2. 404 或 Not Found

Codex 这类 OpenAI 兼容客户端要填 `https://cc.lucoo.net/v1`，少了 `/v1` 就容易报 404。

### 3. No available accounts

可能是分组选错或当前号池繁忙。Codex 主力使用建议优先选 `pro` 分组，稍等一会儿再重试。

### 4. Windows 终端体验不好

Windows 用户建议使用 Windows Terminal 或 WSL2。普通 CMD 可以用，但多窗口、复制粘贴和路径体验会差一些。

## 七、参考入口

- Codex CLI 官方项目：[https://github.com/openai/codex](https://github.com/openai/codex)
- Codex 配置说明：[https://developers.openai.com/codex/config-basic](https://developers.openai.com/codex/config-basic)
- Lucoo 防丢主页：[https://lucoo.net](https://lucoo.net)
