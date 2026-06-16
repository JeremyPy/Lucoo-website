---
title: "腾讯 CodeBuddy 接入 LucooNet API Key 教程"
date: 2026-05-09T11:00:00+08:00
lastmod: 2026-05-09T11:00:00+08:00
description: "从 LucooNet 创建 API Key、正确选择 token 分组，到在腾讯 CodeBuddy 中添加自定义 OpenAI 兼容模型的完整步骤。"
slug: "tencent-codebuddy-lucoonet"
tags: ["CodeBuddy", "AI", "OpenAI", "LucooNet", "中转站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "image-34.png"
---

## 一、先看重点

腾讯 CodeBuddy 支持添加自定义 OpenAI 兼容 API。要接入 LucooNet，核心就是准备好 LucooNet 的 API Key，然后在 CodeBuddy 的模型设置里添加自定义模型。

<p class="lucoo-token-warning-block">最容易出问题的是 API Key 创建时没有选择 token 分组。token 分组必须配置，否则 CodeBuddy 里即使填了 API Key，也可能出现模型不可用、请求失败、无权限调用等问题。</p>

## 二、常用地址

| 用途 | 地址 |
| --- | --- |
| CodeBuddy IDE 下载 | [https://www.codebuddy.cn/cli/](https://www.codebuddy.cn/cli/) |
| LucooNet 中转站 | [https://api.lucoo.net](https://api.lucoo.net) |
| 海外代理访问地址 | [https://apicc.lucoo.net](https://apicc.lucoo.net) |
| 额度购买地址 | [https://pay.ldxp.cn/shop/Lucoo](https://pay.ldxp.cn/shop/Lucoo) |
| 充值地址 | [https://api.lucoo.net/console/topup](https://api.lucoo.net/console/topup) |

## 三、创建 LucooNet API Key

### 1. 登录 LucooNet

打开 [https://api.lucoo.net](https://api.lucoo.net)，按页面提示完成注册并登录。

![登录 LucooNet 中转站](image-1.png)

### 2. 进入令牌管理

登录后进入控制台，打开「令牌管理」，点击「添加令牌」。

![进入令牌管理](image-2.png)

### 3. 必须选择 token 分组

创建令牌时，重点检查「令牌分组」。推荐选择 `plus 号池` 或按你开通情况选择对应的可用分组。

<p class="lucoo-token-warning-block">不要跳过这一步：token 分组必须配置。之前很多人无法使用，就是因为创建 API Key 时没有配置 token 分组，导致后面 CodeBuddy 虽然能保存模型，但实际调用失败。</p>

![创建令牌时选择 token 分组](image-3.png)

建议这样配置：

| 配置项 | 建议 |
| --- | --- |
| 名称 | 随便填写，方便自己识别即可，例如 `CodeBuddy` |
| 令牌分组 | 选择 `plus 号池`、`pro 号池` 或你实际开通的分组 |
| 过期时间 | 可以选择不过期，或按自己的安全要求设置 |
| 额度 | 可保持默认，额度不足时再充值 |
| 模型限制 | 新手建议先不限制，确认可用后再收窄 |

### 4. 复制 API Key

令牌创建成功后，在列表里复制 `sk-` 开头的 API Key。这个 Key 后面要填到 CodeBuddy 里。

不要把真实 API Key 发到公开群聊、截图或代码仓库里。如果需要别人帮你配置，只在可信私聊环境提供；配置完成后也可以重新生成一个新的令牌。

![令牌创建成功并复制 API Key](image-4.png)

## 四、安装并登录 CodeBuddy

### 1. 下载 CodeBuddy IDE

打开 CodeBuddy IDE 下载页，选择 CodeBuddy IDE。

![下载腾讯 CodeBuddy IDE](image-34.png)

### 2. 跳过导入配置

首次启动后可以导入 VS Code 或 Cursor 配置。如果只是按本教程接入 LucooNet，可以先点击「跳过」。

![跳过或导入配置](image-25.png)

### 3. 安装命令行

按页面提示安装 `buddycn` 命令行。

![安装 CodeBuddy 命令行](image-26.png)

安装完成后进入下一步。

![命令行安装完成](image-27.png)

### 4. 登录 CodeBuddy

按 CodeBuddy 页面提示完成登录。

![登录 CodeBuddy](image-28.png)

登录后会进入 CodeBuddy 主界面。

![CodeBuddy 登录后界面](image-29.png)

### 5. 选择工作模式

选择需要的工作模式，然后点击开始工作。

![选择 CodeBuddy 工作模式](image-30.png)

进入工作界面后再配置模型。

![CodeBuddy 工作界面](image-31.png)

## 五、在 CodeBuddy 中添加 LucooNet 模型

### 1. 打开 Claw 设置

点击左下角头像，进入「Claw 设置」。

![进入 Claw 设置](image-32.png)

### 2. 进入模型设置

在设置页面点击「模型」。

![进入模型设置](image-33.png)

### 3. 添加模型

点击「添加模型」。如果弹窗默认显示腾讯云 Coding Plan，不要直接填 API Key，先切换为自定义。

![点击添加模型](image-35.png)

### 4. 选择自定义模型

提供商选择「自定义 / Custom」。

![选择自定义模型](image-36.png)

### 5. 填写 LucooNet API 信息

按下面配置填写：

| 配置项 | 填写内容 |
| --- | --- |
| 提供商 | `自定义 / Custom` |
| 接口地址 | 国内默认填 `https://api.lucoo.net/v1` |
| 海外代理地址 | 如果国内地址不可用，可改填 `https://apicc.lucoo.net/v1` |
| API Key | 粘贴 LucooNet 里复制的 `sk-` 开头令牌，且该令牌必须已配置 token 分组 |
| 模型名称 | 按你可用的模型填写，示例：`gpt-5.5` |
| 高级配置 | 可按截图勾选工具调用、图片输入、推理模式 |

<p class="lucoo-token-warning-block">这里再次确认一次：API Key 必须来自已经配置好 token 分组的令牌。token 分组必须配置，没有 token 分组的 Key 后面大概率无法正常调用。</p>

![填写 LucooNet API 信息](image-37.png)

填写完成后点击「保存」。

![模型配置完成](image-38.png)

## 六、刷新并选择模型

配置保存后，CodeBuddy 有时不会立刻显示新模型。可以点击 Claw，再切换到「新建任务」，相当于刷新一下界面。

![刷新 CodeBuddy 模型列表](image-39.png)

看到刚刚添加的自定义模型后，选中它。

![选择 LucooNet 自定义模型](image-40.png)

选中后就可以在 CodeBuddy 里使用 LucooNet API 进行代码开发或日常办公。

![使用 LucooNet 模型](image-41.png)

## 七、常见问题排查

### 1. 保存成功但模型不能用

优先检查 LucooNet 后台的令牌是否配置了 <span class="lucoo-token-warning">token 分组必须配置</span>。这是最常见的问题。

如果分组为空、选错分组，或者分组没有对应模型权限，CodeBuddy 侧配置看起来正常，但实际请求会失败。

### 2. 提示 API Key 无效

重新复制 LucooNet 后台的完整 `sk-` 开头令牌，确认没有多复制空格，也没有少复制字符。

### 3. 提示模型不可用或无权限

检查你填写的模型名称是否属于当前 token 分组可用范围。新手可以先不要设置模型限制，确认跑通后再限制模型。

### 4. 看不到刚添加的模型

切换到 Claw 的「新建任务」刷新界面，或者完全退出 CodeBuddy 后重新打开。

### 5. 不会配置 API Key

如果不会创建 API Key、不会选择 token 分组，或者不确定 CodeBuddy 里应该怎么填，可以联系 Lucoo 协助配置。不要在公开群聊里直接发送完整 API Key。
