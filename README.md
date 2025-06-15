# Jupyter 使用指南（适用于 Cursor 编辑器）

本项目推荐在 [Cursor](https://www.cursor.so/) 编辑器中使用 Jupyter 笔记本进行算法开发。以下是详细的操作步骤：

## 目录
- [环境配置](#环境配置)
- [本地使用](#本地使用)
- [远程连接](#远程连接)
- [常见问题](#常见问题)

## 环境配置

### 1. 安装 Python 环境
```bash
# 创建虚拟环境
python -m venv .environment

# 激活虚拟环境
# Windows
.environment\Scripts\activate
# macOS/Linux
source .environment/bin/activate
```

### 2. 安装必要的包
```bash
# 安装 Jupyter 相关包
pip install jupyter notebook jupyterlab ipykernel

# 安装文件同步工具
pip install jupytext

# 安装其他常用包
pip install numpy pandas matplotlib
```

### 3. 配置 Jupyter
```bash
# 生成配置文件
jupyter notebook --generate-config

# 设置密码（可选）
jupyter notebook password
```

## 本地使用

### 1. 启动 Jupyter 服务器
```bash
# 启动 Jupyter Notebook
jupyter notebook

# 或启动 JupyterLab
jupyter lab
```

### 2. 在 Cursor 中使用
1. **打开 .ipynb 文件**
   - 使用 `Cmd+P`（Mac）或 `Ctrl+P`（Windows/Linux）搜索并打开文件
   - 或直接双击文件打开

2. **运行代码**
   - 点击单元格左侧的运行按钮
   - 或使用快捷键 `Shift+Enter`

3. **文件同步**
   - 编辑 .ipynb 文件时会自动同步到同名 .py 文件
   - 支持双向同步，方便版本管理

## 远程连接

### 1. 快速开始
1. **在 Cursor 中连接远程服务器**
   - 按 `Cmd+Shift+P`（Mac）或 `Ctrl+Shift+P`（Windows/Linux）
   - 输入 `Jupyter: Specify Jupyter server for connections`
   - 选择 "Existing" 并输入服务器地址（例如：`http://localhost:8888`）
   - 输入密码或 token

2. **使用 SSH 端口转发（推荐）**
   ```bash
   ssh -N -L 8888:localhost:8888 user@remote_host
   ```

### 2. 详细文档
- [远程服务器设置指南](doc/jupyter_server_setup.md)
  - 服务器环境配置
  - Jupyter 服务器设置
  - 安全配置
  - 服务器维护

- [本地客户端连接指南](doc/jupyter_client_setup.md)
  - 本地环境准备
  - 连接方法（SSH 端口转发、Cursor 连接）
  - 安全建议
  - 故障排除

## 常见问题

### 1. 环境问题
- **找不到 Python 解释器**
  - 确保虚拟环境已激活
  - 在 Cursor 设置中指定正确的 Python 路径

- **包安装失败**
  - 检查网络连接
  - 尝试使用国内镜像源：
    ```bash
    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 包名
    ```

### 2. 连接问题
- **无法连接到远程服务器**
  - 检查 SSH 端口转发是否成功
  - 验证服务器地址和端口是否正确
  - 确认密码或 token 是否有效

- **连接不稳定**
  - 使用 `ServerAliveInterval` 保持连接
  - 检查网络连接质量
  - 考虑使用 VPN 或专用网络

### 3. 文件同步问题
- **同步失败**
  - 检查文件权限
  - 确认 jupytext 配置正确
  - 尝试手动同步：
    ```bash
    jupytext --sync 你的文件.ipynb
    ```

## 参考资源

- [Cursor 编辑器](https://www.cursor.so/)
- [Jupyter 官方文档](https://jupyter.org/documentation)
- [Jupytext 文档](https://jupytext.readthedocs.io/)

## 维护说明

- 定期更新依赖包
- 保持配置文件备份
- 及时处理安全更新

如有更多问题，欢迎提 issue 或联系维护者。 