# ChatGPT 无需手机号短信验证使用 Codex 客户端：文章发布设计

## 目标

把根目录中的教程草稿整理为一篇可直接发布的 Hugo 图文教程，标题使用“ChatGPT 无需手机号短信验证使用 Codex客户端”。文章说明：用户先在浏览器中正常登录自己的 ChatGPT 账号，再利用现有登录态生成 `auth.json`，导入 Cockpit 后启动 Codex 客户端；此流程不代表绕过 ChatGPT 本身的登录验证。

## 内容结构

1. 用一段简短说明交代适用场景和最终效果。
2. 列出准备条件：已登录的 ChatGPT 账号、浏览器、Cockpit、Codex 客户端。
3. 按实际界面拆分操作步骤：打开工具、复制 Session JSON、生成并下载 `auth.json`、导入 Cockpit、启动 Codex。
4. 添加认证信息安全说明：Session、Access Token 和 `auth.json` 不得分享、截图公开或上传网盘。
5. 添加常见问题与参考入口，链接到 Codex 工具页和现有 Cockpit 本地反代教程。

## Hugo 页面设计

- 页面包：`content/postsinfo/chatgpt-codex-no-sms-login/`
- 正文：`index.md`
- Slug：`chatgpt-codex-no-sms-login`
- 分类：`技术分享`
- 标签：`ChatGPT`、`Codex`、`Cockpit`、`auth.json`
- 开启目录与摘要。
- 发布时间和更新时间使用 `2026-07-22`。

## 图片处理

- 第一张现有截图作为工具页面总览图，重命名为 `image-01.png`。
- 第二张图使用用户新提供并确认可发布的截图原图，不再裁剪或二次脱敏。
- 图片使用描述性替代文本，不把认证值抄写到正文或图片元数据。

## 发布与验证

1. 用 Hugo 生产模式构建并检查无报错。
2. 确认生成路径为 `/postsinfo/chatgpt-codex-no-sms-login/`，正文链接和图片均存在。
3. 只提交本次文章、图片及必要文档，不提交无关未跟踪文件。
4. 推送到 `origin/main`，等待现有站点发布链路完成。
5. 访问线上文章，检查 HTTP 状态、标题、正文、图片和链接。
