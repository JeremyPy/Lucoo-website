# ChatGPT 无需手机号短信验证使用 Codex 客户端 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 将现有草稿和两张截图整理成 Hugo 页面包，构建验证后提交并推送到 `origin/main`，再检查线上页面。

**Architecture:** 使用站点现有的 Hugo leaf bundle 结构，在 `content/postsinfo/chatgpt-codex-no-sms-login/` 中放置 `index.md` 与两张页面资源图片。正文只引用相对资源路径，发布依赖现有 `main` 分支部署链路，不改主题或全局配置。

**Tech Stack:** Hugo 0.154.5、Markdown、Git、curl

---

### Task 1: 创建文章页面包与图片资源

**Files:**
- Create: `content/postsinfo/chatgpt-codex-no-sms-login/image-01.png`
- Create: `content/postsinfo/chatgpt-codex-no-sms-login/image-02.png`
- Source: `image/chatgpt登录无需接收短信使用codex/1784695178331.png`
- Source: `/var/folders/_m/39mdxlj51v5d_6rfcsddp61c0000gn/T/codex-clipboard-63cda587-e1c3-4161-a5d3-dafdd98be4b4.png`

- [ ] **Step 1: 验证页面包尚不存在**

Run:

```bash
test ! -e content/postsinfo/chatgpt-codex-no-sms-login
```

Expected: exit code `0`。

- [ ] **Step 2: 创建目录并复制用户确认的图片**

```bash
mkdir -p content/postsinfo/chatgpt-codex-no-sms-login
cp image/chatgpt登录无需接收短信使用codex/1784695178331.png \
  content/postsinfo/chatgpt-codex-no-sms-login/image-01.png
cp /var/folders/_m/39mdxlj51v5d_6rfcsddp61c0000gn/T/codex-clipboard-63cda587-e1c3-4161-a5d3-dafdd98be4b4.png \
  content/postsinfo/chatgpt-codex-no-sms-login/image-02.png
```

- [ ] **Step 3: 验证两张图片可以识别**

Run:

```bash
file content/postsinfo/chatgpt-codex-no-sms-login/image-01.png \
     content/postsinfo/chatgpt-codex-no-sms-login/image-02.png
```

Expected: 两行均包含 `PNG image data`。

### Task 2: 写入润色后的 Hugo 正文

**Files:**
- Create: `content/postsinfo/chatgpt-codex-no-sms-login/index.md`

- [ ] **Step 1: 验证正文尚不存在**

Run:

```bash
test ! -e content/postsinfo/chatgpt-codex-no-sms-login/index.md
```

Expected: exit code `0`。

- [ ] **Step 2: 写入完整正文**

```markdown
---
title: "ChatGPT 无需手机号短信验证使用 Codex客户端"
date: 2026-07-22T12:58:22+08:00
lastmod: 2026-07-22T12:58:22+08:00
description: "利用浏览器中已有的 ChatGPT 登录状态生成 auth.json，并导入 Cockpit 启动 Codex 客户端，避免在 Codex 中重复接收手机号短信验证码。"
slug: "chatgpt-codex-no-sms-login"
tags: ["ChatGPT", "Codex", "Cockpit", "auth.json"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "image-01.png"
---

如果你已经能在浏览器中正常使用 ChatGPT，但在 Codex 客户端登录时不方便再次接收手机号短信验证码，可以利用浏览器里已有的登录状态生成 `auth.json`，再通过 Cockpit 导入并启动 Codex。

> 本文所说的“无需手机号短信验证”，是指无需在 Codex 客户端里重新完成一次手机号短信验证。第一次登录 ChatGPT 时，仍需按照账号原本支持的方式正常登录。

<!-- more -->

## 一、准备工作

开始前，请准备好：

- 一个可以正常使用的 ChatGPT 账号；
- 已经登录该账号的浏览器；
- [Cockpit](https://github.com/jlcodes99/cockpit-tools/releases)；
- 已安装的 Codex 客户端；
- `auth.json` 生成工具：[https://codex.lucoo.net/](https://codex.lucoo.net/)。

整个流程可以概括为：

```text
登录 ChatGPT → 复制 Session JSON → 生成 auth.json → 导入 Cockpit → 启动 Codex
```

## 二、生成 auth.json

### 1. 打开生成工具

访问：

[https://codex.lucoo.net/](https://codex.lucoo.net/)

页面会按照“登录、复制 Session、生成、导入 Cockpit”的顺序展示全部操作。

![Codex Agent Identity 生成工具页面](image-01.png)

### 2. 登录 ChatGPT

点击页面中的“打开 chatgpt.com 登录”。

如果当前浏览器已经登录 ChatGPT，可以直接进入下一步；如果还没有登录，请先按照 ChatGPT 的正常流程完成登录，并保持当前浏览器的登录状态。

### 3. 复制 Session JSON

点击“打开 Session 页”，或者在同一个浏览器中访问：

[https://chatgpt.com/api/auth/session](https://chatgpt.com/api/auth/session)

页面正常时会显示一整段以 `{` 开头的 JSON。全选并复制整个页面内容即可，也可以只复制其中的 `accessToken` 字段。

这里必须使用已经登录 ChatGPT 的同一个浏览器，否则可能无法读取到当前账号的 Session。

### 4. 粘贴并生成 auth.json

回到 [Codex Agent Identity](https://codex.lucoo.net/) 页面，将刚刚复制的 Session JSON 或 `accessToken` 粘贴到输入框中，然后点击“生成 auth.json”。

页面解析成功后，会显示当前账号信息和生成结果。

### 5. 下载 auth.json

点击“下载 auth.json”，把生成的文件保存到本地。后面导入 Cockpit 时会用到这个文件。

![生成并下载 auth.json](image-02.png)

## 三、在 Cockpit 中导入 auth.json

### 1. 下载并安装 Cockpit

打开 Cockpit Releases 页面：

[https://github.com/jlcodes99/cockpit-tools/releases](https://github.com/jlcodes99/cockpit-tools/releases)

按照自己的操作系统下载并安装。macOS 通常选择 `.dmg`，Windows 通常选择 `.msi`。

如果还没有使用过 Cockpit，可以先参考：[Cockpit 本地反代与多账号管理教程](https://lucoo.net/postsinfo/cockpit-local-reverse-proxy/)。

### 2. 导入 auth.json

打开 Cockpit，进入 Codex 模块，然后：

1. 点击添加 Codex 账号；
2. 在弹窗顶部选择“导入”；
3. 点击“从本地文件导入”；
4. 选择刚刚下载的 `auth.json`；
5. 解析成功后，确认显示的是 Agent Identity 账号；
6. 点击“不检测，直接导入”。

### 3. 刷新账号状态

导入成功后，在账号卡片底部点击刷新按钮，等待 Cockpit 拉取账号额度和订阅状态。

如果暂时没有显示额度，可以稍等片刻后再刷新一次。

### 4. 启动 Codex

账号状态刷新完成后，点击账号卡片上的“开始”或播放按钮。Cockpit 会使用刚导入的 Agent Identity 账号启动 Codex 客户端，无需再次在 Codex 中走浏览器 OAuth 或短信接码流程。

## 四、常见问题

### Session 页面没有显示 JSON

先确认当前浏览器已经登录 ChatGPT，并且打开 Session 页面时使用的是同一个浏览器和同一个浏览器配置文件。确认后刷新页面再试。

### 生成 auth.json 失败

重新复制完整的 Session 页面内容，确保没有漏掉开头或结尾。如果完整 JSON 无法识别，也可以只复制 `accessToken` 字段重新生成。

### Cockpit 导入失败

确认选择的是刚刚下载的 `auth.json`，并检查文件是否完整。导入时应识别为 Agent Identity 类型。

### 启动后仍然是旧账号

先完全退出 Codex、VS Code 以及其他正在使用 Codex 的程序，再回到 Cockpit 刷新账号并重新点击“开始”。

## 五、安全提醒

`Session JSON`、`accessToken` 和 `auth.json` 都属于账号认证信息：

- 不要发送到公开群聊；
- 不要上传到公开代码仓库或公共网盘；
- 不要交给不可信的人；
- 不再使用时，及时删除本地多余副本；
- 如果怀疑认证信息已经泄露，请立即退出相关账号会话并重新生成认证文件。

## 六、相关入口

- auth.json 生成工具：[https://codex.lucoo.net/](https://codex.lucoo.net/)
- ChatGPT：[https://chatgpt.com/](https://chatgpt.com/)
- Cockpit 下载：[https://github.com/jlcodes99/cockpit-tools/releases](https://github.com/jlcodes99/cockpit-tools/releases)
- Cockpit 完整教程：[https://lucoo.net/postsinfo/cockpit-local-reverse-proxy/](https://lucoo.net/postsinfo/cockpit-local-reverse-proxy/)
```

- [ ] **Step 3: 验证 Front Matter 与引用资源**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
p = Path('content/postsinfo/chatgpt-codex-no-sms-login/index.md')
s = p.read_text()
assert s.startswith('---\n')
assert 'title: "ChatGPT 无需手机号短信验证使用 Codex客户端"' in s
assert 'slug: "chatgpt-codex-no-sms-login"' in s
assert '](image-01.png)' in s
assert '](image-02.png)' in s
assert 'https://codex.lucoo.net/' in s
assert 'https://lucoo.net/postsinfo/cockpit-local-reverse-proxy/' in s
print('article checks passed')
PY
```

Expected: `article checks passed`。

### Task 3: 构建并检查生成页面

**Files:**
- Generated: `public/postsinfo/chatgpt-codex-no-sms-login/index.html`

- [ ] **Step 1: 运行 Hugo 生产构建**

Run:

```bash
hugo --gc --minify
```

Expected: exit code `0`，输出中 `ERROR` 为 `0`。

- [ ] **Step 2: 检查生成页面的标题、图片和链接**

Run:

```bash
python3 - <<'PY'
from pathlib import Path
p = Path('public/postsinfo/chatgpt-codex-no-sms-login/index.html')
s = p.read_text()
for text in [
    'ChatGPT 无需手机号短信验证使用 Codex客户端',
    'image-01.png',
    'image-02.png',
    'https://codex.lucoo.net/',
    'https://lucoo.net/postsinfo/cockpit-local-reverse-proxy/',
]:
    assert text in s, text
print('rendered page checks passed')
PY
```

Expected: `rendered page checks passed`。

- [ ] **Step 3: 检查两张生成图片可以访问到构建目录**

Run:

```bash
test -s public/postsinfo/chatgpt-codex-no-sms-login/image-01.png
test -s public/postsinfo/chatgpt-codex-no-sms-login/image-02.png
```

Expected: 两条命令均为 exit code `0`。

### Task 4: 提交、推送并验证线上页面

**Files:**
- Add: `content/postsinfo/chatgpt-codex-no-sms-login/index.md`
- Add: `content/postsinfo/chatgpt-codex-no-sms-login/image-01.png`
- Add: `content/postsinfo/chatgpt-codex-no-sms-login/image-02.png`

- [ ] **Step 1: 仅暂存本次文章页面包**

```bash
git add content/postsinfo/chatgpt-codex-no-sms-login/index.md \
        content/postsinfo/chatgpt-codex-no-sms-login/image-01.png \
        content/postsinfo/chatgpt-codex-no-sms-login/image-02.png
git diff --cached --stat
```

Expected: 只列出上述三个文件。

- [ ] **Step 2: 提交文章**

```bash
git commit -m "docs: publish ChatGPT Codex no-SMS login guide"
```

Expected: 创建一个包含正文与两张图片的提交。

- [ ] **Step 3: 推送 main 分支**

```bash
git push origin main
```

Expected: `main -> main`。

- [ ] **Step 4: 轮询线上页面并验证内容**

Run:

```bash
for i in {1..18}; do
  body="$(curl -fsSL --max-time 20 https://lucoo.net/postsinfo/chatgpt-codex-no-sms-login/ 2>/dev/null || true)"
  if printf '%s' "$body" | grep -Fq 'ChatGPT 无需手机号短信验证使用 Codex客户端'; then
    printf '%s\n' 'online article verified'
    exit 0
  fi
  sleep 10
done
printf '%s\n' 'online article not visible within 180 seconds' >&2
exit 1
```

Expected: `online article verified`。
