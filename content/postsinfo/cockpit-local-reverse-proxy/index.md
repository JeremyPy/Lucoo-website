---
title: "Cockpit 本地反代与多账号管理教程"
date: 2026-05-14T10:00:00+08:00
lastmod: 2026-05-14T10:00:00+08:00
description: "从下载安装 Cockpit，到配置 Lucoo 中转站 API Key、ChatGPT 账号登录、多账号集合、本地反代和常见问题排查的完整新手教程。"
slug: "cockpit-local-reverse-proxy"
tags: ["Cockpit", "Codex", "OpenAI", "中转站", "本地反代"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "1778770298105.png"
---

Cockpit 是一个本地反代和多账号管理工具，可以在本机统一管理 API Key、ChatGPT 账号、多个账号集合和不同 AI 编程工具的启动配置。常见用途包括一键切号、多账号管理、多开实例、配额监控、唤醒任务、设备指纹、插件联动，以及 GitHub Copilot、Windsurf、Kiro、Cursor、Gemini CLI、CodeBuddy、Qoder、Trae、Zed 等工具的账号管理。

项目地址：[https://github.com/jlcodes99/cockpit-tools](https://github.com/jlcodes99/cockpit-tools)

## 一、先看适用场景

如果你只是想让 Codex 使用官方 ChatGPT 账号，直接在 Cockpit 里添加 ChatGPT 账号即可。

如果你想使用 Lucoo 中转站 API、统一管理额度、或者把多个 API Key/账号放在一起自动切换，建议按本文顺序操作：

1. 准备 Lucoo 中转站 API Key。
2. 安装 Cockpit。
3. 在 Cockpit 中添加 Custom API 账号。
4. 启动本地反代并测试 Codex。
5. 按需添加 ChatGPT 账号和多账号集合。

## 二、常用地址

| 用途 | 地址 |
| --- | --- |
| Cockpit GitHub | [https://github.com/jlcodes99/cockpit-tools](https://github.com/jlcodes99/cockpit-tools) |
| Cockpit 下载页 | [https://github.com/jlcodes99/cockpit-tools/releases/tag/v0.23.4](https://github.com/jlcodes99/cockpit-tools/releases/tag/v0.23.4) |
| Lucoo 中转站 | [https://cc.lucoo.net](https://cc.lucoo.net) |
| 海外代理访问地址 | [https://apicc.lucoo.net](https://apicc.lucoo.net) |
| 额度购买地址 | [https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo) |
| 充值地址 | [https://cc.lucoo.net/console/topup](https://cc.lucoo.net/console/topup) |

国内环境优先使用 `https://cc.lucoo.net/v1` 作为 API 请求地址；如果跨境访问不稳定，可以改用 `https://apicc.lucoo.net/v1`。

## 三、准备 Lucoo 中转站 API Key

如果你已经有可用的 `sk-` 开头 API Key，可以跳过本节，直接看下一节安装 Cockpit。没有 Key 的用户建议先完成下面步骤。

### 1. 注册并登录中转站

打开 [https://cc.lucoo.net](https://cc.lucoo.net)，按页面提示完成注册并登录。

![中转站注册登录页面](lucoo-token-register.png)

### 2. 进入令牌管理

登录后进入后台，找到「令牌管理」，然后点击添加令牌。

![进入令牌管理并添加令牌](lucoo-token-management.png)

### 3. 创建令牌并配置 token 分组

创建令牌时一定要配置 token 分组，选择 Plus 号池、Pro 号池，或者你实际开通的可用分组。

<p class="lucoo-token-warning-block">重点：token 分组必须配置。没有配置 token 分组时，后面 Cockpit 或 Codex 里看起来可能保存成功，但实际请求模型时容易失败。</p>

不建议使用默认 Free 号池。Free 号池额度较少，稳定性和可用模型范围通常不适合长期使用。如果你购买了 Pro 或 Plus 额度，就选择对应分组。

![创建令牌并配置 token 分组](lucoo-token-create.png)

### 4. 复制 API Key

令牌创建成功后，复制 `sk-` 开头的 API Key。后面在 Cockpit 的 Custom 配置里会用到。

![令牌创建成功并复制 API Key](lucoo-token-created.png)

安全提醒：

- 不要把真实 API Key 截图发到公开群聊、网站或代码仓库。
- 如果需要别人远程协助配置，只在可信私聊环境提供，配置完成后也可以重新生成新令牌。
- 如果怀疑 Key 泄露，立即到中转站后台删除旧令牌并重新创建。

## 四、安装 Cockpit

### 1. 下载对应系统版本

打开 Cockpit 下载页：[https://github.com/jlcodes99/cockpit-tools/releases/tag/v0.23.4](https://github.com/jlcodes99/cockpit-tools/releases/tag/v0.23.4)

macOS 用户一般下载 `.dmg` 文件；Windows 用户一般下载 `.msi` 文件。

![选择对应版本下载](1778770123061.png)

### 2. 完成安装

下载后按系统提示完成安装。安装完成后打开 Cockpit。

![完成安装](1778770142841.png)

### 3. 进入首页

打开后可以看到多个工具分类。本文主要以 Codex 为例演示本地反代和多账号配置。

![Cockpit 首页](1778770298105.png)

## 五、配置 Lucoo API Key 到 Cockpit

这一段是中转站 API 接入 Cockpit 的核心步骤。只要 API Key、Base URL 和 Provider Name 填对，后面 Cockpit 就能帮你启动 Codex 并切换到对应账号。

### 1. 进入 Codex 模块

在 Cockpit 首页点击「Codex」。

![点击 Codex](1778770336365.png)

### 2. 选择 API Key 方式

进入 Codex 页面后，点击「API Key」。

![点击 API Key](1778770358510.png)

### 3. 选择 Custom 并填写信息

选择「Custom」，然后按下面方式填写：

| 配置项 | 推荐填写 |
| --- | --- |
| API Key | 粘贴你在 Lucoo 中转站复制的 `sk-` 开头令牌 |
| Base URL | 国内默认填 `https://cc.lucoo.net/v1` |
| 海外代理 Base URL | 如果国内地址不可用，改填 `https://apicc.lucoo.net/v1` |
| Provider Name | 建议填 `Lucoo`，方便后面识别 |

再次确认：这个 API Key 必须来自已经配置好 token 分组的令牌。没有分组或分组权限不匹配时，保存后也可能无法正常调用模型。

![选择 Custom 并填写 API Key](1778770465564.png)

### 4. 添加账号

确认填写无误后点击「Add Account」。

添加完成后，页面会新增一个 `Lucoo` 模块。

![新增 Lucoo 模块](1778770579599.png)

### 5. 启动并写入 Codex 配置

点击账号卡片上的开始按钮。Cockpit 会自动弹出 Codex 客户端，并完成本地配置。

![启动 Codex 客户端](1778770626262.png)

### 6. 测试是否可用

进入 Codex 后发起一次简单测试。如果页面能正常响应、链接正常打开，说明 Lucoo API Key 已经接入成功。

![Codex 测试正常](1778770649206.png)

## 六、添加 ChatGPT 账号

除了 API Key，Cockpit 也支持直接添加 ChatGPT 账号。适合已经有 ChatGPT Plus、Team 或其他账号的用户。

### 1. 点击新增账号

回到 Codex 账号页面，点击新增按钮。

![新增 ChatGPT 账号入口](1778770736220.png)

### 2. 在浏览器打开登录

点击「在浏览器打开」。

![在浏览器打开](1778770807180.png)

### 3. 登录 ChatGPT

这一步和正常登录 ChatGPT 一样，可以使用账号密码、邮箱验证码等方式完成登录。如果没有账号，可以通过店铺地址购买后再登录。

![登录 ChatGPT](1778770875832.png)

### 4. 等待授权成功

登录完成后，浏览器页面会出现授权成功提示。

![授权成功](1778770895452.png)

### 5. 返回 Cockpit

回到 Cockpit 后，可以看到账号已经登录成功。

![Cockpit 登录提示](1778770917336.png)

### 6. 查看账号额度

账号添加成功后，Cockpit 会展示账号额度。后续点击开始按钮，就可以把 Codex 切换到这个 ChatGPT 账号。

Cockpit 在启动时还会自动修复一些常见会话问题，减少切号后会话异常的情况。

![账号切换并修复会话](1778771039173.png)

打开 Codex 后，可以看到已经切换到刚刚登录的账号。

![Codex 切换到 ChatGPT 账号](1778771079766.png)

## 七、配置多账号集合与自动切换

如果你有多个账号，建议使用集合管理。这样可以把多个账号放在一个集合里，让本地反代按需要切换渠道。

### 1. 添加账号到集合

点击添加账号。

![添加账号到集合](1778771149299-redacted.png)

### 2. 选择账号并保存集合

选择刚刚添加的账号，然后保存集合。右上角的 Free 账号限制选项保持默认即可。

![选择账号并保存集合](1778771177295.png)

### 3. 继续添加更多账号

如果你有更多 ChatGPT 账号或 API Key，可以继续添加。没有代理环境的小伙伴需要自行准备代理；如果不想自己维护，也可以使用中转站 API 的方式统一管理额度。

### 4. 启动集合

添加完成后点击开始。

![启动集合](1778771299509-redacted.png)

![确认启动集合](1778771307865.png)

继续点击启动。

![继续启动](1778771324260.png)

Cockpit 会继续修复会话并完成 Codex 启动。

![完成 Codex 启动](1778771348169.png)

### 5. 确认本地服务名称

启动成功后，服务名称会显示为 `codex api service`。

![codex api service](1778771383462.png)

### 6. 再次测试

在 Codex 中发起一次测试，确认可以正常使用。

![集合测试正常](1778771400058.png)

### 7. 查看额度

回到 Cockpit 查看额度情况。如果额度正常变化，说明请求已经走到了当前账号或当前渠道。

![查看额度情况](1778771485833.png)

当某个账号额度不够时，可以在 Cockpit 中切换渠道，或者切回 Lucoo 中转站 API Key。

## 八、常用功能

Cockpit 不只是切号工具，下面这些功能也很常用。

### 1. 会话管理

如果之前的会话丢了、找不到了，可以进入会话管理查看。

![会话管理](1778771626640.png)

### 2. 唤醒任务

如果有定时任务需要 AI 处理，可以使用唤醒任务。这里可以自定义提示词，让工具在指定场景自动唤醒。

![唤醒任务](1778771726960.png)

### 3. 兼容其他平台

Cockpit 还兼容其他平台，例如 Google 相关工具和其他 AI 编程工具。

![兼容其他平台](1778771761488.png)

### 4. 导出账号配置

如果账号需要给别人使用，也可以通过导出功能分享配置。导出前注意确认里面是否包含敏感信息。

![导出账号配置](1778771840706.png)

## 九、常见问题

### 1. API Key 添加成功但 Codex 不能用

优先检查下面几项：

1. API Key 是否复制完整，前后不要有空格。
2. token 分组是否已经配置，且分组有对应模型权限。
3. Base URL 是否带 `/v1`，例如 `https://cc.lucoo.net/v1`。
4. 国内网络是否能访问中转站；如果不稳定，尝试 `https://apicc.lucoo.net/v1`。
5. 修改配置后是否完全退出并重新打开 Codex。

### 2. 启动后仍然是旧账号

Codex 或编辑器可能还保留旧会话缓存。先完全退出 Codex、VS Code 或其他调用工具，再通过 Cockpit 重新启动。

### 3. 多账号集合没有自动切换

先确认集合里已经加入多个可用账号，并且这些账号额度正常。然后回到 Cockpit 查看当前启动的是单个账号还是集合服务。

### 4. 额度不够了怎么办

可以选择：

1. 在 Cockpit 中切换到其他账号或其他渠道。
2. 使用 Lucoo 中转站 API Key，并到 [https://cc.lucoo.net/console/topup](https://cc.lucoo.net/console/topup) 充值。
3. 购买新的额度后，再回到中转站绑定或充值。

### 5. 不会配置中转站 API

记住最核心的三项：

| 配置项 | 应该填什么 |
| --- | --- |
| API Key | Lucoo 中转站令牌管理里复制的 `sk-` 开头令牌 |
| Base URL | `https://cc.lucoo.net/v1` 或 `https://apicc.lucoo.net/v1` |
| token 分组 | 创建令牌时必须选择 Plus、Pro 或你实际开通的可用分组 |

这三项正确后，再回到 Cockpit 选择 Custom 添加账号即可。

## 十、安全提醒

- API Key 和 ChatGPT 登录态都属于账号凭证，不要公开分享。
- 上传教程截图前，确认 API Key 已经打码或隐藏。
- 账号导出文件可能包含敏感配置，只发给可信的人。
- 如果账号或 Key 出现异常，优先删除旧令牌、重新创建令牌，再重新添加到 Cockpit。
- 购买或充值后，到 [https://cc.lucoo.net/console/topup](https://cc.lucoo.net/console/topup) 完成兑换或充值确认。
