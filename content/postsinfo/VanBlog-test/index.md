---
title: "VanBlog - 一个简单的博客系统"
date: 2026-01-30
lastmod: 2026-01-30
description: "一款简洁实用优雅的高性能个人博客系统。"
slug: "VanBlog-test"
tags: ["博客", "网站"]
categories: ["技术分享"]
showTableOfContents: true
showSummary: true
featureimage: "cover.png"
---

部署基于 Docker 的应用，如 VanBlog，通常快捷方便。然而，一个小小的配置错误就可能导致整个项目解析失败。本文将详细解析一个常见的 Docker Compose 错误——`service "mongo" refers to undefined volume ${PWD}/data/mongo: invalid compose project`，并提供一个稳定可靠的解决方案，确保您的服务成功运行。

<!-- more -->
## 💡[官方手册](https://vanblog.mereith.com/guide/get-started.html#%E4%BB%8B%E7%BB%8D)

## 🔍 问题现象：Docker Compose 解析失败

当您尝试使用以下命令启动服务时：

```bash
docker compose up -d
```

控制台可能会提示如下错误：

```
parsing failed: failed to load project: service "mongo" refers to undefined volume ${PWD}/data/mongo: invalid compose project
```

这个错误指向您的 `docker-compose.yml` 文件中的一个特定配置项：MongoDB 服务的数据卷定义。

## 💡 根源分析：${PWD} 变量的陷阱

在 Docker Compose 配置中，我们经常使用 **绑定挂载（Bind Mounts）** 将宿主机的本地文件夹映射到容器内部，用于持久化数据（如数据库文件、上传的静态文件等）。

在您的配置中，`mongo` 服务的卷定义如下：

```yaml
  mongo:
    # ...
    volumes:
      - ${PWD}/data/mongo:/data/db
```

### 错误发生的原因：

1.  \*\*${PWD} 解析问题：** `${PWD}`是一个 Unix/Linux Shell 变量，代表 “Print Working Directory”（当前工作目录）。虽然 Docker Compose 理论上支持 shell 变量替换，但在某些旧版本或特定环境下（尤其是在 Windows/PowerShell 环境或使用旧版`docker-compose\` CLI 时），它可能无法正确解析。
2.  **被误识别为命名卷：** 当变量替换失败或解析不正确时，Docker Compose 可能会将完整的字符串 `${PWD}/data/mongo` 视为一个**命名卷（Named Volume）** 的名称。由于这个名称没有在文件顶层的 `volumes:` 部分声明，因此系统会抛出 **"refers to undefined volume"** 的错误。

## ✅ 完美解决方案：使用相对路径 `./`

最简单、最稳定、最兼容的解决方案是**避免使用 Shell 变量 `${PWD}`**，直接使用 **相对路径 `./`** 来指定本地目录。

相对路径 `./` (代表当前目录) 在 Docker Compose 中作为绑定挂载的源路径时，总是能被正确识别，因为它明确地指向了文件系统中的一个本地路径，而不是一个命名对象。

### 📌 关键修改点

将配置文件中所有使用 `${PWD}` 的地方，全部替换为 `./`。

例如，将：

```yaml
# 错误写法
- ${PWD}/data/static:/app/static
- ${PWD}/data/mongo:/data/db
```

修改为：

```yaml
# 正确写法 (推荐)
- ./data/static:/app/static
- ./data/mongo:/data/db
```

## 🛠️ 成功的 VanBlog 部署配置

以下是修正后的、可成功部署的 `docker-compose.yml` 文件。请确保您的本地目录结构中包含 `data/static`、`log`、`caddy/config`、`caddy/data` 等文件夹，或者在启动时由 Docker Compose 自动创建。

```yaml
version: '3'

services:
  vanblog:
    image: mereith/van-blog:latest
    restart: always
    environment:
      TZ: 'Asia/Shanghai'
      # ⚠️ 邮箱地址必须填写，用于自动申请 HTTPS 证书
      EMAIL: 'your-email@example.com' 
    volumes:
      # 图床文件存储，使用相对路径 `./`
      - ./data/static:/app/static
      # 日志文件存储，使用相对路径 `./`
      - ./log:/var/log
      # Caddy 配置存储
      - ./caddy/config:/root/.config/caddy
      # Caddy 证书存储
      - ./caddy/data:/root/.local/share/caddy
    ports:
      # 宿主机端口:容器端口
      - 80:80
      - 443:443
  
  mongo:
    # 推荐使用 4.4 版本，兼容性更好
    image: mongo:4.4.16 
    restart: always
    environment:
      TZ: 'Asia/Shanghai'
    volumes:
      # MongoDB 数据持久化，使用相对路径 `./` 避免解析错误
      - ./data/mongo:/data/db 
```

### 部署步骤总结

1.  **创建文件：** 将上述内容保存为 `docker-compose.yml` 文件。
2.  **修改邮箱：** 将 `EMAIL` 环境变量替换为您自己的真实邮箱地址，以便 Caddy 自动申请 SSL/TLS 证书。
3.  **运行命令：** 在 `docker-compose.yml` 文件所在的目录下执行启动命令。

<!-- end list -->

```bash
docker compose up -d
```

现在，您的 VanBlog 容器和 MongoDB 容器应该能够顺利启动，并且数据将持久化到本地的 `./data/mongo` 和 `./data/static` 目录中。

-----