# 他乡遇故知

个人技术博客，基于 [Hexo](https://hexo.io/) 静态站点生成器构建，使用 [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly) 主题，部署在 GitHub Pages。

- 线上地址：https://xz613.github.io
- 仓库地址：https://github.com/xz613/xz613.github.io

## 项目简介

本站主要记录 Java 后端学习过程中的笔记，涵盖 Java 基础、Spring 生态、数据库、中间件以及开发工具等内容。文章以 Markdown 编写，通过 Hexo 生成静态页面后发布。

## 技术栈

| 类别 | 技术 |
|------|------|
| 静态站点生成 | [Hexo](https://hexo.io/) 7.x |
| 博客主题 | [Butterfly](https://github.com/jerryc127/hexo-theme-butterfly) |
| 模板引擎 | EJS / Pug |
| 样式 | Stylus |
| Markdown 渲染 | marked |
| 运行环境 | Node.js 20+ |
| 部署 | GitHub Actions + GitHub Pages |

### 主要插件

- `hexo-generator-archive` — 归档页
- `hexo-generator-category` — 分类页
- `hexo-generator-tag` — 标签页
- `hexo-generator-index` — 首页
- `hexo-wordcount` — 字数统计
- `hexo-lazyload-image` — 图片懒加载
- `hexo-server` — 本地开发服务器

## 目录结构

```
.
├── source/                 # 站点源文件
│   ├── _posts/             # 博客文章（Markdown）
│   ├── _data/              # 数据文件（如友链 link.yml）
│   ├── about/              # 关于页面
│   ├── link/               # 友情链接页面
│   ├── music/              # 音乐页面
│   ├── movies/             # 电影页面
│   └── img/                # 图片资源
├── themes/butterfly/       # Butterfly 主题
├── scaffolds/              # 新建文章/页面的模板
├── _config.yml             # Hexo 主配置
├── _config.butterfly.yml   # Butterfly 主题配置（覆盖主题默认配置）
├── package.json
└── .github/workflows/      # GitHub Actions 自动部署
```

## 环境要求

- [Node.js](https://nodejs.org/) >= 20（与 CI 环境保持一致）
- [npm](https://www.npmjs.com/)（或 yarn / pnpm）

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/xz613/xz613.github.io.git
cd xz613.github.io
```

### 2. 安装依赖

```bash
npm install
```

### 3. 本地预览

```bash
npm run server
```

启动后访问 http://localhost:4000 查看效果。

### 4. 构建静态文件

```bash
npm run build
```

构建产物输出到 `public/` 目录。

### 5. 清理缓存

```bash
npm run clean
```

## 常用命令

| 命令 | 说明 |
|------|------|
| `npm run server` | 启动本地开发服务器 |
| `npm run build` | 生成静态站点到 `public/` |
| `npm run clean` | 清除缓存和构建产物 |
| `npm run deploy` | 通过 Hexo 部署到 `view` 分支（备用方式） |

## 写文章

使用 Hexo 命令新建文章：

```bash
npx hexo new "文章标题"
```

文章会生成在 `source/_posts/` 目录下，编辑 Markdown 内容后保存即可。Front Matter 示例：

```yaml
---
title: 文章标题
date: 2025-03-11 12:00:00
tags:
  - Java
categories:
  - Java
---
```

## 内容分类

当前文章主要按以下分类组织：

| 分类 | 内容 |
|------|------|
| Java | JavaSE、Spring、Spring Boot 等 |
| 数据库 | MySQL、MyBatis |
| 中间件 | Redis |
| 工具 | Git、SVN |

## 配置文件说明

| 文件 | 作用 |
|------|------|
| `_config.yml` | 站点标题、URL、permalink、主题选择等全局配置 |
| `_config.butterfly.yml` | 导航菜单、侧边栏、搜索、社交链接等主题配置 |
| `source/_data/link.yml` | 友情链接数据 |

修改配置后，重新运行 `npm run server` 或 `npm run build` 即可生效。

## 部署方式

### 自动部署（推荐）

将代码 **推送到 `main` 分支** 后，会自动触发 GitHub Actions 部署，无需手动执行构建命令。

工作流配置文件：`.github/workflows/pages.yml`

触发条件：

```yaml
on:
  push:
    branches:
      - main
```

自动执行流程：

1. 安装依赖（`npm install`）
2. 构建站点（`npm run build`）
3. 将 `public/` 目录部署到 GitHub Pages

部署完成后，站点会更新到 https://xz613.github.io ，通常 1～3 分钟内生效。

#### 前提条件

在 GitHub 仓库 **Settings → Pages** 中，需要将 **Build and deployment → Source** 设置为 **GitHub Actions**，而不是 `view` 等分支。

> 若 Source 仍指向 `view` 分支，线上会显示旧版内容（例如作者仍为 John Doe、标签/分类为 0），与本地预览不一致。

#### 部署失败排查

若 push 后站点未更新，请到 **Actions** 页查看 **Pages** 工作流是否成功。常见原因：

1. **Pages 源未切换到 GitHub Actions**（见上文）
2. **工作流执行失败**（构建报错时需根据日志修复）
3. **浏览器缓存**（可强制刷新或无痕模式访问）

#### 注意事项

- **仅 `main` 分支会触发自动部署**，推送到其他分支不会执行该工作流。
- 可在仓库 **Actions** 页查看 **Pages** 工作流运行状态，绿色勾表示部署成功。
- 日常更新博客只需：`git add` → `git commit` → `git push origin main`。

### 手动部署（备用）

项目还保留了 Hexo 内置的 Git 部署配置，将构建结果推送到 `view` 分支：

```bash
npm run deploy
```

该方式 **不会在 push 时自动执行**，需手动运行命令。当前推荐使用上方的 GitHub Actions 自动部署，日常维护 push 到 `main` 即可。

部署配置见 `_config.yml` 中的 `deploy` 字段。

## 主题特性

Butterfly 主题已启用以下功能：

- 中文导航菜单
- 本地搜索
- 代码高亮（Mac 风格 + 复制按钮）
- 文章目录（TOC）
- 侧边栏（作者卡片、最近文章、分类、标签云）
- 字数统计
- 图片懒加载
- 深色模式

更多主题配置请参考 [Butterfly 官方文档](https://butterfly.js.org/)。

## 许可证

博客文章内容为个人学习笔记。Butterfly 主题遵循其自身的开源许可证，详见 `themes/butterfly/LICENSE`。
