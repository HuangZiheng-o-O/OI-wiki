如果您更喜欢使用 Conda 和 VSCode/JetBrains，以下是基于您的偏好修改的 OI Wiki 本地部署步骤：

### Conda 环境部署和开发配置

1. **克隆 GitHub 仓库：**
   ```bash
   git clone https://github.com/OI-wiki/OI-wiki.git --depth=1
   ```

2. **进入项目目录：**
   ```bash
   cd OI-wiki
   ```

3. **创建 Conda 环境：**
   使用 Conda 创建一个新的虚拟环境并激活它：
   ```bash
   conda create -n oi-wiki python=3.9 -y
   conda activate oi-wiki
   ```

4. **安装依赖：**
   使用 Conda 和 pip 安装所需的依赖项：
   ```bash
   pip install mkdocs==1.5.3
   pip install pymdown-extensions==10.3.1
   pip install pygments
   pip install beautifulsoup4
   pip install requests
   pip install python-markdown-document-offsets-injection-extension==0.5.12
   pip install markdown==3.6
   
   ```
   
5. **安装自定义主题：**
   执行以下命令来安装 OI Wiki 的自定义主题：
   ```bash
   ./scripts/pre-build/install-theme.sh
   ```

6. **启动本地服务器：**
   有两种方式预览或构建项目：
   - **方式一：** 启动本地服务器，访问 `http://127.0.0.1:8000` 查看效果：
     ```bash
     mkdocs serve -v
     ```
   - **方式二：** 构建静态页面到 `site` 文件夹：
     ```bash
     mkdocs build -v
     ```

7. **使用 VSCode 或 JetBrains IDE 进行开发：**
   - **VSCode：**
     - 安装 Python 插件，确保其使用 Conda 环境。
     - 打开项目文件夹后，按 `Ctrl+Shift+P`，选择 `Python: Select Interpreter` 并选择 `oi-wiki` 环境。
   - **JetBrains IDE（如 PyCharm）：**
     - 在项目设置中配置 Python 解释器，选择 Conda 环境路径。
     - 可选：配置项目运行配置，直接运行 `mkdocs serve` 命令以便调试。

### FAQ
- **为什么选 Conda？** Conda 更加适合管理不同 Python 版本和科学计算库，且兼容性较强。
- **如何切换主题或调整配置？** 直接编辑 `mkdocs.yml` 文件，并使用支持 YAML 语法的编辑器插件查看配置效果。

如需进一步调整步骤，请告诉我！