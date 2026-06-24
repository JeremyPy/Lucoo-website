---
title: "OpenCode 接入 Lucoo 中转站教程"
date: 2026-06-24T10:03:00+08:00
lastmod: 2026-06-24T10:03:00+08:00
description: "在 OpenCode 中添加 Lucoo OpenAI 兼容供应商，使用自己的 API Key 调用 Codex/GPT 模型。"
slug: "opencode-lucoo-relay"
tags: ["OpenCode", "AI", "CLI", "中转站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

## 一、准备信息

OpenCode 支持自定义模型供应商。Lucoo 按 OpenAI 兼容接口接入时，核心只需要三项：

| 配置项 | 填写内容 |
| --- | --- |
| Provider 名称 | `lucoo` |
| Base URL | `https://cc.lucoo.net/v1` |
| API Key | Lucoo 后台「API 密钥」里创建的 `sk-` 开头 Key |

主站不稳定时，备用 Base URL 可以换成：

- `https://api.lucoo.net/v1`
- `https://hkcc.lucoo.net/v1`
- `https://sgcc.lucoo.net/v1`
- `https://uscc.lucoo.net/v1`

## 二、安装 OpenCode

先按 OpenCode 官方文档安装。安装后确认命令可用：

```bash
opencode --version
```

如果命令不存在，说明安装没有成功，先处理 OpenCode 本身的安装问题，再配置 Lucoo。

## 三、准备 API Key

1. 打开 [https://cc.lucoo.net](https://cc.lucoo.net)。
2. 在「钱包」兑换余额。
3. 在「API 密钥」创建 Key。
4. 分组建议选择 `pro`，轻量使用可选 `plus`。

不要把兑换码填进 OpenCode。OpenCode 只能填 API Key。

## 四、编辑 OpenCode 配置

OpenCode 的全局配置文件通常在：

```text
~/.config/opencode/opencode.json
```

如果文件不存在，先创建目录：

```bash
mkdir -p ~/.config/opencode
```

写入或合并下面的 provider 配置：

```json
{
  "$schema": "https://opencode.ai/config.json",
  "provider": {
    "lucoo": {
      "npm": "@ai-sdk/openai",
      "name": "Lucoo",
      "options": {
        "baseURL": "https://cc.lucoo.net/v1"
      },
      "models": {
        "gpt-5.5": {
          "name": "gpt-5.5",
          "reasoning": true,
          "modalities": {
            "input": ["text", "image", "pdf"],
            "output": ["text"]
          }
        }
      }
    }
  }
}
```

如果你的 OpenCode 版本要求 OpenAI 兼容包，也可以把 `npm` 改成：

```json
"npm": "@ai-sdk/openai-compatible"
```

## 五、配置 API Key

最简单的方式是在当前终端里设置环境变量：

```bash
export OPENAI_API_KEY="sk-这里填你自己的 Lucoo API Key"
```

Windows PowerShell：

```powershell
$env:OPENAI_API_KEY="sk-这里填你自己的 Lucoo API Key"
```

如果你希望长期生效，可以把环境变量写到自己的 shell 配置里。不要把真实 Key 写进公开仓库。

## 六、启动并选择模型

启动 OpenCode：

```bash
opencode
```

在模型或 provider 选择里找到 Lucoo，然后选择 `gpt-5.5` 或后台当前可用模型。

测试提示词：

```text
请用三句话说明你当前连接的模型供应商和可用能力。
```

## 七、排查

### 1. 提示 provider 不存在

检查 `opencode.json` 是否是合法 JSON，尤其不要漏逗号、花括号。

### 2. 提示 API Key 错误

确认 `OPENAI_API_KEY` 是 `sk-` 开头的 Key，不是兑换码。

### 3. 请求失败或 404

确认 Base URL 是 `https://cc.lucoo.net/v1`，末尾带 `/v1`。

### 4. 模型没有列出来

手动把模型名写到 `models` 里，或者回后台确认当前 Key 的分组是否支持该模型。

## 八、参考入口

- OpenCode 配置文档：[https://opencode.ai/docs/config/](https://opencode.ai/docs/config/)
- OpenCode providers 文档：[https://opencode.ai/docs/providers/](https://opencode.ai/docs/providers/)
- Lucoo 防丢主页：[https://lucoo.net](https://lucoo.net)
