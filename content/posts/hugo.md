---
date: '2025-02-18T14:00:37+08:00'
draft: true
title: '使用Hugo部署Github pages'
tags: ["教程"]
description: "本教程由AI生成"
draft: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
---

使用 [Hugo](w) 在 [GitHub Pages](w) 上部署静态网站涉及以下步骤，包括安装 Hugo、创建站点、添加主题、生成静态文件，并通过 GitHub Actions 进行自动化部署。  

---

## **1. 安装 Hugo**
在本地环境安装 Hugo：

- **macOS（Homebrew）**
  ```sh
  brew install hugo
  ```

- **Windows（Scoop）**
  ```sh
  scoop install hugo
  ```

- **Linux（APT）**
  ```sh
  sudo apt install hugo
  ```

可以通过以下命令检查安装是否成功：
```sh
hugo version
```

---

## **2. 创建 Hugo 站点**
```sh
hugo new site my-hugo-site
cd my-hugo-site
```

---

## **3. 添加主题**
Hugo 主题通常托管在 GitHub 上，你可以通过 `git submodule` 或直接克隆它们：

```sh
git init
git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke
```

然后在 `config.toml` 中启用主题：
```toml
theme = "ananke"
```

---

## **4. 添加内容**
创建新文章：
```sh
hugo new posts/my-first-post.md
```
编辑 `content/posts/my-first-post.md` 并设置 `draft = false` 以使其可见。

---

## **5. 预览网站**
使用 `hugo server` 预览：
```sh
hugo server -D
```
访问 `http://localhost:1313/` 进行查看。

---

## **6. 生成静态文件**
```sh
hugo
```
生成的静态文件位于 `public/` 目录。

---

## **7. 配置 GitHub Pages**
在 GitHub 上创建一个新的仓库（如 `my-hugo-site`），然后连接远程仓库：
```sh
git remote add origin https://github.com/your-username/my-hugo-site.git
```

---

## **8. 设定 GitHub Actions 自动部署**
在 `my-hugo-site` 仓库的 **GitHub Pages** 设置中，选择 `gh-pages` 作为发布分支。

然后，在 `my-hugo-site` 的 `.github/workflows/gh-pages.yml` 文件中添加以下 CI/CD 配置：

```yaml
name: Deploy Hugo site to GitHub Pages

on:
  push:
    branches:
      - main  # 触发自动部署的分支

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          submodules: true  # 拉取主题
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: "latest"

      - name: Build
        run: hugo --minify

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

然后提交并推送：
```sh
git add .
git commit -m "Initial commit"
git push origin main
```

---

## **9. 访问 GitHub Pages**
如果你的仓库名是 `your-username.github.io`，那么你的 Hugo 站点将托管在：
```
https://your-username.github.io/
```
否则，你可以在 GitHub Pages 设置中找到访问 URL。

---

## **10. 更新站点**
每次更新内容后：
```sh
git add .
git commit -m "Update content"
git push origin main
```
GitHub Actions 会自动重新部署。

---

这样，你的 Hugo 站点就能通过 GitHub Pages 自动构建并发布 🚀！
