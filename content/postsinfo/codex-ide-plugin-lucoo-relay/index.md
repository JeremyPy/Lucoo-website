---
title: "VS Code / Cursor / Trae Codex 插件接入 Lucoo 教程"
date: 2026-06-24T10:01:00+08:00
lastmod: 2026-06-24T10:01:00+08:00
description: "在 VS Code、Cursor、Trae 等编辑器里使用 Codex 插件时，配置 Lucoo 中转站 API Key 的新手教程。"
slug: "codex-ide-plugin-lucoo-relay"
tags: ["Codex", "VS Code", "Cursor", "Trae", "中转站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

## 一、适用范围

这篇适合已经在 VS Code、Cursor、Trae 这类编辑器里安装 Codex 插件，但想把请求切到 Lucoo 中转站的用户。

Codex CLI 和 Codex IDE 插件共享配置层。也就是说，大多数情况下你只要把 `~/.codex/config.toml` 和 `~/.codex/auth.json` 配好，编辑器重启后就会读取同一套配置。

![IDE 插件读取 Codex 本地配置流程](ide-config-flow.png)

## 二、准备 Lucoo API Key

1. 打开 [https://cc.lucoo.net](https://cc.lucoo.net)。
2. 先到「钱包」兑换余额。
3. 再到「API 密钥」创建 `sk-` 开头的 Key。
4. Codex 插件建议使用 `pro` 分组，轻量体验可用 `plus` 分组。

<p class="lucoo-token-warning-block">不要把兑换码填到插件里。插件里要填的是后台创建出来的 API Key。</p>

## 三、打开 Codex 配置文件

如果编辑器里有 Codex 设置入口，可以点击右上角齿轮，选择 Codex Settings，再打开 `config.toml`。

也可以直接在终端执行：

```bash
mkdir -p ~/.codex
open ~/.codex
```

Windows 用户可以打开：

```text
C:\Users\你的用户名\.codex
```

如果目录不存在，就手动新建 `.codex` 文件夹。

## 四、写入 auth.json

在 `.codex` 目录里创建 `auth.json`：

```json
{
  "OPENAI_API_KEY": "sk-这里填你自己的 Lucoo API Key"
}
```

保存后不要把这个文件发给别人。

## 五、写入 config.toml

在同一个目录里创建 `config.toml`：

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

国内默认用主站即可。网络不稳定时，可以把 `base_url` 改成：

```toml
base_url = "https://hkcc.lucoo.net/v1"
```

其他备用入口：

- `https://api.lucoo.net/v1`
- `https://sgcc.lucoo.net/v1`
- `https://uscc.lucoo.net/v1`

## 六、重启编辑器

配置文件保存后，请完整退出 VS Code、Cursor 或 Trae，再重新打开。

只关闭窗口不一定够，建议确认程序已经完全退出。很多 401、仍然走官方接口的问题，本质都是编辑器还在读旧缓存。

## 七、常见报错

### 1. 配置后还是 401

优先检查两件事：

1. `auth.json` 是否存在，里面是不是 `sk-` 开头的 API Key。
2. `config.toml` 是否保存到了当前系统用户的 `.codex` 目录。

如果还是不行，重新创建一个 API Key 再试。

### 2. 插件仍然跳官方登录

说明插件没有读到你的配置。重启编辑器后再打开 Codex 插件；如果你在远程 SSH / WSL 里开发，要注意 `.codex` 目录要放在远程环境的用户目录里。

### 3. 模型名不存在

先用后台可见模型名，不要手打猜测。模型、分组和倍率会调整，以后台显示为准。

## 八、客户排查时怎么发截图

如果需要协助，请提供：

- 使用的是 VS Code、Cursor 还是 Trae。
- 当前填写的 Base URL。
- 模型名。
- 完整报错截图。
- 注册邮箱或订单编号。

不要直接发完整 API Key。确实需要验证时，建议新建一个临时限额 Key，处理完立刻删除。

## 九、参考入口

- Codex 配置基础：[https://developers.openai.com/codex/config-basic](https://developers.openai.com/codex/config-basic)
- Codex 高级配置：[https://developers.openai.com/codex/config-advanced](https://developers.openai.com/codex/config-advanced)
- Lucoo 防丢主页：[https://lucoo.net](https://lucoo.net)
