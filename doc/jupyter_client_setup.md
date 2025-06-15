# Jupyter 本地客户端连接指南

本文档详细说明如何从本地计算机连接到远程 Jupyter 服务器。

## 一、准备工作

### 1. 获取远程服务器信息
- 服务器 IP 地址
- Jupyter 服务端口（默认 8888）
- 访问密码或 token

### 2. 确保本地环境
```bash
# 安装必要的包
pip install jupyter notebook jupyterlab
```

## 二、连接方法

### 1. SSH 端口转发（推荐）

#### 基本转发
```bash
ssh -N -L 8888:localhost:8888 user@remote_host
```
- `user`: 远程服务器用户名
- `remote_host`: 远程服务器地址

#### 保持连接
```bash
ssh -N -L 8888:localhost:8888 user@remote_host -o ServerAliveInterval=60
```

#### 多端口转发（如果需要）
```bash
ssh -N -L 8888:localhost:8888 -L 8889:localhost:8889 user@remote_host
```

### 2. 在 Cursor 中连接

1. **打开命令面板**
   - 按 `Cmd+Shift+P`（Mac）或 `Ctrl+Shift+P`（Windows/Linux）
   - 输入 `Jupyter: Specify Jupyter server for connections`

2. **选择连接方式**
   - 选择 "Existing"
   - 输入 `http://localhost:8888`

3. **输入认证信息**
   - 输入远程服务器的密码或 token
   - token 通常在远程服务器启动时在终端显示

### 3. 在浏览器中连接

1. **打开浏览器**
   - 访问 `http://localhost:8888`

2. **输入认证信息**
   - 输入密码或 token

## 三、连接验证

### 1. 检查端口转发
```bash
# 检查本地端口是否在监听
netstat -an | grep 8888
```

### 2. 测试连接
```bash
# 测试到远程服务器的连接
curl http://localhost:8888
```

## 四、常见问题

### 1. 连接被拒绝
- 检查 SSH 端口转发是否成功
- 确认远程服务器是否正在运行
- 验证端口号是否正确

### 2. 认证失败
- 确认密码或 token 是否正确
- 检查远程服务器的认证配置
- 尝试重新获取 token

### 3. 连接不稳定
- 使用 `ServerAliveInterval` 保持连接
- 检查网络连接质量
- 考虑使用 VPN 或专用网络

## 五、安全建议

### 1. 使用 SSH 密钥
```bash
# 生成 SSH 密钥
ssh-keygen -t rsa -b 4096

# 复制公钥到服务器
ssh-copy-id user@remote_host
```

### 2. 配置 SSH 配置
编辑 `~/.ssh/config`：
```text
Host jupyter-server
    HostName remote_host
    User your_username
    Port 22
    LocalForward 8888 localhost:8888
    ServerAliveInterval 60
```

然后使用简化的命令：
```bash
ssh jupyter-server
```

### 3. 使用 SSL 连接
如果远程服务器配置了 SSL，使用 HTTPS：
```bash
ssh -N -L 8888:localhost:8888 user@remote_host
```
然后在浏览器中使用 `https://localhost:8888`

## 六、性能优化

### 1. 网络优化
- 使用有线网络连接
- 关闭不必要的网络应用
- 考虑使用网络加速器

### 2. 本地配置
- 使用本地缓存
- 配置适当的超时时间
- 优化浏览器设置

## 七、故障排除

### 1. 连接问题
```bash
# 检查 SSH 连接
ssh -v user@remote_host

# 检查端口转发
netstat -an | grep 8888
```

### 2. 重置连接
1. 关闭所有相关进程
2. 重新建立 SSH 连接
3. 重新配置 Jupyter 连接

### 3. 日志查看
- 检查本地 Jupyter 日志
- 查看 SSH 连接日志
- 检查浏览器控制台错误 