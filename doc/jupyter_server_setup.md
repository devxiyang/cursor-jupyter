# Jupyter 远程服务器设置指南

本文档详细说明如何在远程服务器上设置和配置 Jupyter 服务器。

## 一、基础环境配置

### 1. 安装必要的包
```bash
# 创建并激活虚拟环境（推荐）
python -m venv jupyter_env
source jupyter_env/bin/activate

# 安装必要的包
pip install jupyter notebook jupyterlab
```

### 2. 生成配置文件
```bash
jupyter notebook --generate-config
```

### 3. 设置访问密码
```python
# 在 Python 交互环境中运行
from jupyter_server.auth import passwd
passwd()
```
将生成的密码哈希复制到 `~/.jupyter/jupyter_notebook_config.py`：
```python
c.NotebookApp.password = '你的密码哈希'
```

## 二、服务器配置

### 1. 基本配置
编辑 `~/.jupyter/jupyter_notebook_config.py`，添加以下配置：

```python
# 允许远程访问
c.NotebookApp.allow_origin = '*'
c.NotebookApp.allow_credentials = True

# 设置 IP 和端口
c.NotebookApp.ip = '0.0.0.0'  # 允许所有 IP 访问
c.NotebookApp.port = 8888     # 默认端口

# 禁用浏览器自动打开
c.NotebookApp.open_browser = False
```

### 2. 安全配置（可选但推荐）
```python
# SSL 配置（如果有证书）
c.NotebookApp.certfile = '/path/to/cert.pem'
c.NotebookApp.keyfile = '/path/to/key.pem'

# 限制访问 IP（可选）
c.NotebookApp.ip = '你的服务器IP'
```

## 三、启动服务器

### 1. 基本启动命令
```bash
jupyter notebook
```
或使用 JupyterLab：
```bash
jupyter lab
```

### 2. 后台运行（推荐）
使用 `screen` 或 `tmux`：
```bash
# 使用 screen
screen -S jupyter
jupyter notebook
# 按 Ctrl+A 然后按 D 来分离会话

# 或使用 tmux
tmux new -s jupyter
jupyter notebook
# 按 Ctrl+B 然后按 D 来分离会话
```

### 3. 使用 systemd 服务（推荐用于生产环境）
创建服务文件 `/etc/systemd/system/jupyter.service`：
```ini
[Unit]
Description=Jupyter Notebook Server
After=network.target

[Service]
Type=simple
User=你的用户名
WorkingDirectory=/home/你的用户名
ExecStart=/home/你的用户名/jupyter_env/bin/jupyter notebook
Restart=always

[Install]
WantedBy=multi-user.target
```

启动服务：
```bash
sudo systemctl enable jupyter
sudo systemctl start jupyter
```

## 四、防火墙设置

### 1. 开放端口
```bash
# Ubuntu/Debian
sudo ufw allow 8888

# CentOS/RHEL
sudo firewall-cmd --permanent --add-port=8888/tcp
sudo firewall-cmd --reload
```

## 五、验证服务器状态

1. **检查服务是否运行**
```bash
ps aux | grep jupyter
```

2. **检查端口是否开放**
```bash
netstat -tulpn | grep 8888
```

3. **查看日志**
```bash
# 如果使用 systemd
sudo journalctl -u jupyter

# 或者直接查看 Jupyter 日志
jupyter notebook --debug
```

## 六、常见问题

### 1. 端口被占用
修改配置文件中的端口号：
```python
c.NotebookApp.port = 8889  # 使用其他端口
```

### 2. 权限问题
```bash
# 确保配置文件权限正确
chmod 600 ~/.jupyter/jupyter_notebook_config.py
```

### 3. 内存不足
在配置文件中添加内存限制：
```python
c.NotebookApp.mem_limit = 4 * 1024 * 1024 * 1024  # 4GB
```

## 七、维护建议

1. **定期更新**
```bash
pip install --upgrade jupyter notebook jupyterlab
```

2. **日志轮转**
配置 logrotate 来管理日志文件

3. **监控资源使用**
使用 `htop` 或 `top` 监控服务器资源使用情况

4. **备份配置**
定期备份配置文件和工作目录 