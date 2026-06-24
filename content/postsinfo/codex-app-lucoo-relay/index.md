---
title: "Codex App 桌面客户端接入 Lucoo 中转站"
date: 2026-06-24T10:02:00+08:00
lastmod: 2026-06-24T10:02:00+08:00
description: "不想只用命令行时，如何让 Codex App 桌面客户端读取 Lucoo 中转站配置。"
slug: "codex-app-lucoo-relay"
tags: ["Codex App", "Codex", "OpenAI", "中转站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

## 一、先说明一个重要变化

Codex App、Codex CLI 和 IDE 插件的登录方式可能会随着版本变化调整。本文给的是客户最容易操作的本地配置方式：先准备 `~/.codex` 配置目录，再让桌面端读取同一套 API Key 和模型供应商配置。

如果桌面端当前版本提示必须重新登录，请先退出原来的 ChatGPT 账号，再按本文配置。

## 二、准备后台信息

| 项目 | 填写内容 |
| --- | --- |
| 登录后台 | `https://cc.lucoo.net` |
| Codex API 地址 | `https://cc.lucoo.net/v1` |
| API Key | 在 Lucoo 后台「API 密钥」页面创建 |
| 推荐分组 | `pro` 优先，轻量使用可选 `plus` |
| 防丢主页 | `https://lucoo.net` |

<p class="lucoo-token-warning-block">兑换码只用于钱包兑换余额，不是 Codex App 的登录 Key。</p>

## 三、退出旧账号

如果 Codex App 之前登录过 ChatGPT 官方账号，建议先在 App 里退出。

更换到 API Key 方式后，原账号云端历史和本地新会话不是一套空间。为了避免误会，建议先确认自己是否还需要旧账号里的历史记录。

## 四、创建配置目录

macOS / Linux：

```bash
rm -rf ~/.codex
mkdir -p ~/.codex
```

Windows：

```powershell
Remove-Item -Recurse -Force "$env:USERPROFILE\.codex" -ErrorAction SilentlyContinue
New-Item -ItemType Directory "$env:USERPROFILE\.codex"
```

如果你不想删除旧配置，可以先把 `.codex` 文件夹改名备份。

## 五、创建 auth.json

macOS / Linux 路径：

```text
~/.codex/auth.json
```

Windows 路径：

```text
C:\Users\你的用户名\.codex\auth.json
```

内容如下：

```json
{
  "OPENAI_API_KEY": "sk-这里填你在 Lucoo 后台创建的 Key"
}
```

## 六、创建 config.toml

同一个目录里创建 `config.toml`：

```toml
model = "gpt-5.5"
model_provider = "lucoo"
model_reasoning_effort = "xhigh"
sandbox_mode = "workspace-write"
approval_policy = "on-request"
file_opener = "vscode"

[model_providers.lucoo]
name = "Lucoo"
base_url = "https://cc.lucoo.net/v1"
env_key = "OPENAI_API_KEY"
wire_api = "responses"
```

如果网络不稳定，只改 `base_url`：

```toml
base_url = "https://sgcc.lucoo.net/v1"
```

## 七、启动 Codex App

打开 Codex App 后，新建一个会话，先发一个简单问题测试：

```text
用一句话回复：当前配置已经可以正常请求模型。
```

如果能正常返回，就说明 App 已经读取到 Lucoo 配置。

## 八、常见问题

### 1. 打开后还是官方账号

先退出 App，再重新打开。必要时删除 `.codex` 目录重新创建，确认 `auth.json` 和 `config.toml` 文件名没有写成 `auth.json.txt`。

### 2. 提示 API Key 无效

检查是否复制了 `sk-` 开头的 API Key，而不是兑换码。复制时不要带空格。

### 3. 提示模型不可用

回到 Lucoo 后台确认 Key 的分组。Codex 主力使用优先选 `pro`。

### 4. 网络连接失败

把 API 地址换成备用入口：

- `https://api.lucoo.net/v1`
- `https://hkcc.lucoo.net/v1`
- `https://sgcc.lucoo.net/v1`
- `https://uscc.lucoo.net/v1`

## 九、参考入口

- Codex App 入口：[https://openai.com/codex](https://openai.com/codex)
- Codex 配置参考：[https://developers.openai.com/codex/config-reference](https://developers.openai.com/codex/config-reference)
- Lucoo 防丢主页：[https://lucoo.net](https://lucoo.net)
