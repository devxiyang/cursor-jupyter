# Jupyter 使用指南（适用于 Cursor 编辑器）

本项目推荐在 [Cursor](https://www.cursor.so/) 编辑器中使用 Jupyter 笔记本进行算法开发。以下是详细的操作步骤：

---

## 一、在 Cursor 中本地运行 Jupyter 笔记本

1. **确保已安装依赖**
   - 推荐使用虚拟环境（如 `.environment` 或 `venv`）。
   - 安装依赖：
     ```bash
     pip install ipykernel jupytext
     ```

2. **启动 Jupyter 服务器**
   - 在终端运行：
     ```bash
     jupyter notebook
     ```
   - 或者：
     ```bash
     jupyter lab
     ```

3. **在 Cursor 中打开 .ipynb 文件**
   - 直接双击或用 `Cmd+P` 搜索打开。
   - 可以像在 Jupyter Lab 一样运行、编辑、插入单元格。

4. **同步 .ipynb 和 .py 文件**
   - 本项目已配置 [jupytext](https://jupytext.readthedocs.io/en/latest/)。
   - 编辑 .ipynb 文件时，会自动同步到同名 .py 文件，方便版本管理。

---

## 二、在 Cursor 中连接远程 Jupyter 服务器

1. **在远程服务器上启动 Jupyter**
   ```bash
   jupyter notebook --no-browser --port=8888
   # 或
   jupyter lab --no-browser --port=8888
   ```

2. **本地 SSH 端口转发**
   ```bash
   ssh -N -L 8888:localhost:8888 user@remote_host
   ```
   - `user` 替换为你的远程用户名
   - `remote_host` 替换为远程服务器地址

3. **在 Cursor 中指定远程 Jupyter 服务器**
   - 按 `Cmd+Shift+P`，输入 `Jupyter: Specify Jupyter server for connections`，选择"Existing"。
   - 输入 `http://localhost:8888`，并根据提示输入 token 或密码。

4. **打开/新建 .ipynb 文件即可远程运行**

---

## 常见问题

- **如何生成 Jupyter 密码？**
  在远程服务器 Python 交互环境中运行：
  ```python
  from jupyter_server.auth import passwd
  passwd()
  ```
  按提示输入密码，复制生成的 hash，写入 `~/.jupyter/jupyter_notebook_config.py`：
  ```python
  c.NotebookApp.password = '你的hash'
  ```

- **如何查看 Jupyter token？**
  启动 Jupyter 后，终端会输出一串带 `?token=...` 的 URL，复制 token 即可。

---

如有更多问题，欢迎提 issue 或联系维护者。 