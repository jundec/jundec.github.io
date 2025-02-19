---
title: "JupyterLab的服务器部署"
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
description: "在conda环境下JupyterLab的服务器部署"
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

这里，`--name your_env_name` 用来指定内核的名字（可以任意取名），`--display-name "Python (your_env_name)"` 用来指定在 Jupyter 中显示的名称。

### 步骤 5：回到base环境

```bash
conda deactivate
```

---

通过以上步骤，现在可以在 JupyterLab 打开的页面中选择 `Python (your_env_name)` 的 Python 内核。
下面在服务器上配置 JupyterLab 并通过端口转发在本地访问：

### 步骤 6：生成配置文件

```bash
jupyter-lab --generate-config
```
配置文件会生成在 `~/.jupyter/jupyter_lab_config.py`。

### 步骤 7：修改配置文件

编辑生成的配置文件，修改以下关键配置：
```python
# 允许远程访问
c.ServerApp.allow_remote_access = True
# 监听所有 IP 地址
c.ServerApp.ip = '0.0.0.0'
# 设置端口（默认 8888，可自定义）
c.ServerApp.port = 8888
# 禁止自动打开浏览器
c.ServerApp.open_browser = False
# 设置密码或禁用密码（可选）
c.ServerApp.password = ''  # 留空则通过 Token 访问
c.ServerApp.token = ''     # 留空则禁用 Token
```
### 步骤 8：设置密码（可选但推荐）

生成密码哈希：
```bash
jupyter-lab password
```
输入密码后，配置文件会自动更新 `c.ServerApp.password`。

### 步骤 9：启动 Jupyter Lab

在服务器后台启动：
```bash
nohup jupyter-lab --no-browser > jupyter.log 2>&1 &
```
或使用 `screen`/`tmux` 保持会话。

### 步骤 10：本地 SSH 端口转发

通过 SSH 将服务器的端口（如 `8888`）转发到本地：
```bash
ssh -N -L localhost:8888:localhost:8888 user@server_ip
```
- `-N`: 不执行远程命令。
- `-L`: 本地端口转发。

也可以编辑 `~/.ssh/config` 文件，将远程服务器的端口映射到本地端口：
```bash
Host my-server-alias          # 自定义别名（任意名称）
    HostName server.example.com # 服务器 IP 或域名
    User username               # 登录用户名
    LocalForward 8888 127.0.0.1:8888  # 格式: 本地端口:远程目标地址:远程端口
```
之后使用 SSH 进行连接：
```bash
ssh my-server-alias # 直接使用别名连接，自动启用端口转发
```

### 步骤 11：处理防火墙（如有）

确保服务器防火墙开放端口（如 `8888`）：
```bash
sudo ufw allow 8888  # 如果使用 ufw
```
云服务器需在安全组中放行端口。

### 步骤 12：本地访问

在本地浏览器输入：
```
http://localhost:8888
```
- 如果设置了密码，输入密码。
- 如果未设置密码，检查服务器日志 `jupyter.log` 获取 Token。

通过以上步骤，可以在服务器运行 JupyterLab，并通过 SSH 在本地访问。

