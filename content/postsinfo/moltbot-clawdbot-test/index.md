---
title: "clawdbot/moltbot/OpenClaw 安装 + Telegram 接入（新手一步一步）"
date: 2026-01-30
lastmod: 2026-01-30
description: "从服务器准备到 Telegram Bot 接入，全流程实操记录"
slug: "openclaw-telegram-setup"
tags: ["教程", "AI", "Bot"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

> 这篇是我按自己的实际安装流程写的，偏小白视角，跟着做基本不会出错。

## 0. 名字是怎么来的（可略读）

项目名字经历过三次变化：

- **Clawd**（2025 年 11 月）—— 取自 “Claude + claw”，但被 Anthropic 提醒过商标问题
- **Moltbot** —— Discord 早上 5 点社区脑暴的产物，“蜕壳成长”的寓意不错，但不好念
- **OpenClaw** —— 最终定名，商标/域名都确认过，迁移已完成

我理解的含义是：

- **Open**：开源、社区驱动、对所有人开放
- **Claw**：延续项目“龙虾钳”文化

GitHub：
https://github.com/openclaw/openclaw

---

## 1. 先准备服务器（建议配置）

我这次用的是 **Ubuntu 24.04**，配置建议如下：

- **CPU**：2 核以上
- **内存**：2 GB 以上（可以加 swap）
- **硬盘**：30 GB SSD
- **网络**：正常能联网即可

> 我实际用的是新加坡 2G 机器，能跑，但建议还是 2C2G 更舒服。

---

## 2. 安装 OpenClaw（两种方式）

**官方推荐方式：**

Linux：
```bash
curl -fsSL https://openclaw.bot/install.sh | bash
```

Windows（PowerShell）：
```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

**我实际用的方式（npm 全局安装）：**

```bash
npm install -g openclaw@latest
```

安装后执行：

```bash
openclaw onboard --install-daemon
```

这条命令的意思：

- `onboard`：首次初始化流程
- `--install-daemon`：作为后台常驻服务（类似 systemd）
  - 开机自启
  - 长期运行
  - 能持续接收消息

安装截图：

![安装 OpenClaw](image/index/1769757077782.png)

---

## 3. 安全提示（这个必须看）

进入引导后会看到安全提示页：

![安全提示](image/index/1769757419029.png)

核心意思是：**这是 Beta 项目，能力强但风险高**。

官方建议你至少做到：

✅ **allowlist / mention gating**
- 限制能调用哪些工具
- 防止提示词诱导危险操作

✅ **sandbox + 最小权限**
- 不要用 root（我这里是 root，只是新机测试）
- 尽量用普通用户 / Docker / sandbox

✅ **不要把 secrets 放在可读目录**
- `.env` / key / token 不要让它能扫到

✅ **有工具能力时尽量用强模型**
- 低能力模型容易被 prompt 绕过

我这边是新开的 VPS，风险可控，所以直接继续。

界面提示：
“I understand this is powerful and inherently risky. Continue? Yes / No”

我选择 **Yes**。

![确认继续](image/index/1769757811094.png)

---

## 4. 选择快速开始（Quick Start）

因为是第一次安装，直接走快速开始，回车即可：

![快速开始](image/index/1769757946906.png)

---

## 5. 选择模型供应商（以 Z.AI / GLM 为例）

这里会让你选择 AI 模型供应商：

我选择 **智谱 GLM 4.7**，原因：

- 国内访问稳定
- 有免费额度
- 新手上手成本低

截图：

![选择模型](image/index/1769758103948.png)

然后在控制台选择 **Z.AI**：

![选择 Z.AI](image/index/1769758168967.png)

输入 API Key：

![输入 API Key](image/index/1769758222738.png)

默认模型选择 GLM4.7：

![默认模型](image/index/1769758297429.png)

---

## 6. 渠道状态页（解释一下界面）

这里会出现渠道状态页：

![渠道状态](image/index/1769758350699.png)

我把内容翻译整理如下：

### 6.1 Channel status（渠道状态）

| 渠道 | 状态 |
| --- | --- |
| Telegram | 未配置 |
| WhatsApp | 未配置 |
| Discord | 未配置 |
| Google Chat | 未配置 |
| Slack | 未配置 |
| Signal | 未配置 |
| iMessage | 未配置 |
| Google Chat | 需要插件 |
| Nostr | 需要插件 |
| Microsoft Teams | 需要插件 |
| Mattermost | 需要插件 |
| Nextcloud Talk | 需要插件 |
| Matrix | 需要插件 |
| BlueBubbles | 需要插件 |
| LINE | 需要插件 |
| Zalo | 需要插件 |
| Zalo Personal | 需要插件 |
| Tlon | 需要插件 |

### 6.2 How channels work（渠道机制说明）

| 项目 | 说明 |
| --- | --- |
| 私聊（DM）安全机制 | 默认使用 pairing 机制；陌生人私聊会收到配对码 |
| 批准方式 | `openclaw pairing approve <channel> <code>` |
| 公共私聊 | `dmPolicy="open"` + `allowFrom=["*"]` |
| 多用户私聊隔离 | `session.dmScope="per-channel-peer"` |
| 文档 | 参考 start / pairing |

### 6.3 各渠道简单评价

| 渠道 | 说明 |
| --- | --- |
| Telegram | 最简单入门，推荐新手 |
| WhatsApp | 绑定手机号，建议独立手机号 |
| Discord | 支持度非常好 |
| Slack | 支持 Socket Mode |
| Signal | 需要 signal-cli，比较折腾 |
| iMessage | 仍在开发 |
| Nostr | 去中心化协议 |
| Microsoft Teams | 企业向 |
| Matrix | 需插件 |
| BlueBubbles | 需 macOS 配合 |
| LINE / Zalo | 主要针对海外市场 |
| Tlon | Urbit 生态 |

---

## 7. 选择 Telegram（最稳妥）

这里直接选 **Telegram (Bot API)**，回车即可：

![选择 Telegram](image/index/1769758565946.png)

---

## 8. 创建 Telegram Bot（非常详细）

1) 打开 Telegram 搜索 **BotFather**（注意官方图标）

![BotFather](image/index/1769758646999.png)

2) 点进来点击开始

![开始](image/index/1769758695828.png)

3) 输入 `/newbot`

![newbot](image/index/1769758752119.png)

4) 输入机器人名称（显示名）

![机器人名称](image/index/1769758810262.png)

5) 输入机器人用户名（必须以 bot 结尾）

> 例如：`Lucoo_MoltBot_bot`

![用户名](image/index/1769758990165.png)

6) 会返回 Bot Token

**注意：**

![提示](image/index/1769759104533.png)

7) 点你的 Bot 就能开始对话

---

## 9. 回到服务器，粘贴 Token

把 Token 贴回去，回车确认：

![粘贴 Token](image/index/1769759052306.png)

---

## 10. Skills 是什么？（先理解概念）

OpenClaw 的 Skill = AI 的“动手能力模块”。

没有 Skill，它只能聊天；
有 Skill，它可以执行命令、读写文件、调用 API。

官方解释：

```
用户消息
  ↓
LLM（负责想）
  ↓ 选择
Skill（负责做）
  ↓
系统 / API / 外部工具
```

Skill 能做什么？

| 类型 | 举例 |
| --- | --- |
| 系统操作 | 执行命令、重启服务 |
| 文件操作 | 读 / 写文件 |
| 网络请求 | HTTP / API |
| 自动化 | 浏览器操作、脚本执行 |
| 运维集成 | 部署、监控、日志 |

Skill = Tool + 规则 + 限制

| 项目 | Tool | Skill |
| --- | --- | --- |
| 层级 | 底层 | 封装层 |
| 安全性 | 低 | 高 |
| 能力范围 | 很泛 | 很具体 |
| 是否受控 | ❌ | ✅ |

总结：**Skill 决定 AI 能干什么**。

![Skill 界面](image/index/1769759559288.png)

---

## 11. 安装 Skills 的依赖（按提示走）

这里提示 Homebrew 依赖：

![Homebrew 提示](image/index/1769759675867.png)

我这里已经装过，直接回车即可。

接着让你选 3 个安装方式，我选 **npm**，因为 Node.js 已经有了：

![选择 npm](image/index/1769759769373.png)

随后出现大量 Skills（有些是 mac 专用），我这里暂时不装：

![技能列表](image/index/1769759962460.png)

提示必须选一个，我空格选第一个：

![至少选一个](image/index/1769759999574.png)

---

## 12. 依赖型 Skill 的配置提示（可以先跳过）

例如：

**goplaces** 需要 Google Places API Key

![goplaces](image/index/1769760189481.png)

**local-places** 也需要同样的 Key

![local-places](image/index/1769760241114.png)

**nano-banana-pro** 需要 Gemini API Key（用于图像生成）

![nano-banana-pro](image/index/1769760351724.png)

**notion** 需要 Notion API Key

![notion](image/index/1769760414203.png)

如果你暂时用不到，直接回车跳过即可。

---

跳过后会有一个确认提示，我这里直接继续：

![跳过配置提示](image/index/1769760494055.png)

---

## 13. Hooks 说明（可选，但建议了解）

这里会出现 Hooks 选择。我的理解很简单：

- **Hook** = 在某些指令触发时，自动执行后台动作
- 类似 Git hook / CI hook / Webhook

常见的几个：

| Hook 名称 | 作用 | 白话解释 |
| --- | --- | --- |
| `boot-md` | 启动时加载说明 | 启动时先读一段规则 |
| `command-logger` | 记录命令 | 记录你对 Agent 的指令 |
| `session-memory` | 会话记忆 | 保存当前会话到长期记忆 |

我这里全开，看看效果（会消耗一点上下文/token）：

![Hooks 选择](image/index/1769760940916.png)

回车继续，安装完成：

![安装完成](image-2.png)

---

## 14. 常用命令（状态 / 帮助）

查看当前状态：

```bash
openclaw status
```

![状态](image/index/1769761164021.png)

查看帮助：

```bash
openclaw help
```

![帮助](image-3.png)

官方命令文档（可选）：https://docs.openclaw.ai/cli

---

## 15. Web 控制台（Dashboard）

OpenClaw 有一个本地 Dashboard，我这里用 SSH 隧道转发到本地打开：

```bash
ssh -N -L 18789:127.0.0.1:18789 root@你的服务器IP
```

![SSH 隧道](image/index/1769761376292.png)

浏览器打开：

```
http://127.0.0.1:18789
```

如果提示需要带 token 的链接：

![需要 Token](image.png)

把 token 拼进去：

```
http://localhost:18789/?token=你的token
```

正常进入后会显示健康状态：

![控制台正常](image/index/1769761685598.png)

---

## 16. Telegram 不回消息？排查 + 重配流程

先看渠道列表：

```bash
openclaw channels list
```

![渠道列表](image/index/1769762129808.png)

我这边当时遇到 TG `/start` 没反应：

![无反应](image/index/1769762249903.png)
![无反应](image/index/1769762405280.png)
![无反应](image/index/1769762414920.png)

解决思路是 **先删掉旧配置再重配**：

```bash
openclaw channels remove
```

一路回车删除当前配置：

![删除旧配置](image/index/1769762530629.png)

然后重新走一遍 Telegram 配置：

![重新配置](image/index/1769762619011.png)

注意这里一定要选 **Yes**：

![选择 Yes](image/index/1769763307517.png)

后面会让你设置消息模板/配置项：

![配置消息](image/index/1769765370229.png)

---

## 17. autoSelectFamily=false 报错处理

我这边重启后遇到报错：

![报错](image/index/1769765402833.png)

修复方式：

```bash
openclaw config set channels.telegram.network.autoSelectFamily true
pkill -f openclaw
pkill -f node
openclaw gateway --port 18789
```

回到 TG 里输入 `/start`，会给一个配对码：

![配对码](image/index/1769765686425.png)

在服务器执行：

```bash
openclaw pairing approve telegram C2LJXXX
```

（把 `C2LJXXX` 替换成你自己的 code）

之后就能正常对话：

![对话成功](image/index/1769765774418.png)

---

## 结语

到这里，OpenClaw + Telegram 的完整流程就跑通了（包含踩坑部分）。给大家看一下效果：
![1769767052555](image/index/1769767052555.png)