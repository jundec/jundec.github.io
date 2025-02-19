---
title: "JupyterLab部署"
date: 2017-05-15T01:11:15+00:00
# weight: 1
# aliases: ["/first"]
tags: ["教程"]
author: "Me"
# author: ["Me", "You"] # multiple authors
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "在conda环境下部署JupyterLab"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

可以通过以下步骤使用 `conda` 配置 `JupyterLab`，
并确保它可以在 `base` 环境下运行，并且发现 `your_env_name` 环境下的 Python 内核：

### 步骤 1：在 `conda` 的 `base` 环境下安装 `jupyterlab`

```bash
conda install jupyterlab
```

### 步骤 2：激活要配置的环境 `your_env_name`

```bash
conda activate your_env_name
```

### 步骤 3：安装 `ipykernel` 包

在 `your_env_name` 环境下，安装 `ipykernel`，

```bash
conda install ipykernel
```

### 步骤 4：注册 `your_env_name` 环境的 Python 内核

将 `your_env_name` 环境的 Python 内核注册到 Jupyter。运行以下命令：

```bash
python -m ipykernel install --user --name your_env_name --display-name "Python (your_env_name)"
```

这里，`--name your_env_name` 用来指定内核的名字（可以任意取名），`--display-name "Python (your_env_name)"` 用来指定在 Jupyter 中显示的名称，可以忽略，默认使用conda环境名称。

### 步骤 5：启动 JupyterLab

在 JupyterLab 打开的页面中，现在可以选择不同的 Python 内核（包括 `Python (your_env_name)` 内核）。
