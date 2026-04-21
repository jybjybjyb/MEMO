# 常用命令- [常用命令](#常用命令)
- [常用命令- 常用命令](#常用命令--常用命令)
- [命令 (CLI)](#命令-cli)
- [安装](#安装)
  - [一行命令安装 WSL2](#一行命令安装-wsl2)
  - [在配置好的 WSL2 中安装 OpenClaw](#在配置好的-wsl2-中安装-openclaw)
  - [权限](#权限)
  - [代理](#代理)
  - [手动安装 Node.js（绕开拦截，100% 成功率）](#手动安装-nodejs绕开拦截100-成功率)
  - [在线搜索](#在线搜索)
  - [模型](#模型)




# 命令 (CLI)

OpenClaw 的操作主要通过终端命令完成，以下是你日常最可能用到的：

>  openclaw gateway restart
> 
- 1. 基础配置与维护
* **`openclaw onboard`**：最常用的入门命令。启动交互式引导，帮你配置模型（GPT-5, Claude 4 等）、频道和基础权限。
* **`openclaw doctor`**：这是你的“救命稻草”。当搜索不通、API 报错或配置失效时，运行它会进行全系统体检并自动尝试修复。
* **`openclaw configure`**：进入高级配置模式，可以手动调整 `searxng` 地址、更改 LLM 提供商或修改安全级别。
* **`openclaw status`**：查看当前 Agent 的健康状态、已连接的频道（如 Telegram）以及活跃的任务。

- 2. 技能与插件管理
OpenClaw 的强大在于“技能（Skills）”，你可以通过命令扩展它的能力：
* **`openclaw install [skill-name]`**：从官方库（ClawHub）安装技能。例如：`openclaw install web-search-searxng`。
* **`openclaw skills list`**：查看你已经安装了哪些超能力。
* **`openclaw backup`**：备份你的配置和记忆数据库。

- 3. 运行与调试
* **`openclaw dashboard`**：在浏览器中打开可视化控制面板。如果你觉得改 `.env` 太累，可以直接在 UI 里改。
* **`openclaw agent --message "执行任务"`**：直接在终端给 AI 下指令，无需通过聊天软件。


# 安装
## 一行命令安装 WSL2

**1. 以管理员身份运行 PowerShell**
按下 `Win` 键，搜索“**PowerShell**”，然后选择“**以管理员身份运行**”。

**2. 执行安装命令**
在弹出的蓝底窗口中，复制粘贴以下命令并回车：
```powershell
wsl --install
```
> **说明：** 这个命令会自动为你启用必要的 Windows 功能（如虚拟机平台），下载最新的 Linux 内核，并默认安装 **Ubuntu** 系统。你只需要耐心等待进度条走完。

**3. 重启电脑**
安装完成后，PowerShell 会提示你需要重启计算机。请保存好工作并**重启电脑**。

**4. 初始化 Ubuntu（设置账号密码）**
重启后，通常会自动弹出一个黑色的 Ubuntu 终端窗口（如果没有自动弹出，可以在开始菜单搜索“Ubuntu”并打开）。
* 系统会花一两分钟进行最后的解压。
* 接着会提示你输入 `Enter new UNIX username:`（设置一个你喜欢的全英文用户名，不需要和 Windows 用户名一样）。
* 然后提示 `New password:`（输入密码，注意：**输入密码时屏幕上不会显示任何字符**，这是正常的，输入完按回车即可）。
* 再次确认密码。

---

## 在配置好的 WSL2 中安装 OpenClaw



```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

等待它自动下载并配置好环境，最后同样运行 `openclaw onboard` 来启动初始化向导即可。

## 权限
- 完整的 Linux 环境权限（100% 自由）
   在那个黑色的 Ubuntu 终端里，OpenClaw 拥有像在一台独立 Linux 服务器上一样的完整权限：
   * **最高权限 (Root)：** 它可以执行 `sudo` 命令（前提是你授予了权限或免密），安装任何软件包、配置网络、运行后台守护进程。
   * **代码执行：** 当 OpenClaw 的 Agent 需要编写并运行 Python 脚本、Node.js 项目或 Bash 命令时，它在这个环境里畅通无阻，这正是官方推荐使用 Linux 的原因——**兼容性极佳**。

- 访问 Windows 文件的权限（双向互通）
   WSL2 默认会自动将你的 Windows 磁盘挂载到 Linux 系统中。
   * **你的 C 盘在 Linux 里的路径是：** `/mnt/c/`
   * **你的 D 盘在 Linux 里的路径是：** `/mnt/d/`
   **权限状态：** OpenClaw 可以读取、修改甚至删除你 Windows 桌面或文档里的文件。它在访问这些文件时，**拥有的权限等同于你当前登录的 Windows 账户权限**。它不能随意修改 Windows 的核心系统文件（比如 `C:\Windows\System32`），但可以完全掌控你的个人文件。

- 调用 Windows 程序的权限（跨系统执行）
   WSL2 有一个特性叫 **WSL Interop（互操作性）**。
   * 这意味着 OpenClaw 在 Ubuntu 里，可以直接输入命令来调用 Windows 上的 `.exe` 程序。
   * 例如，它可以调用 Windows 的浏览器、打开记事本，或者执行 Windows 环境下的脚本。

- 硬件访问权限（GPU 满血输出）
   如果你打算让 OpenClaw 在本地运行开源大语言模型（比如配合 Ollama）：
   * WSL2 支持 **GPU 直通 (GPU Passthrough)**。
   * 只要你的 Windows 安装了最新的显卡驱动，WSL 里的 OpenClaw 就能直接调用你的 NVIDIA 显卡进行 CUDA 加速，几乎没有性能损耗。


- 安全建议

   把 OpenClaw 放在 WSL2 里，既给了它发挥最大能力的舞台，又提供了一层**软隔离**：

   * **好处：** 如果 Agent 在执行任务时把环境搞崩溃了（比如装错了依赖、写了死循环），死掉的只是那个 Ubuntu 系统。你的 Windows 11 本体会安然无恙。大不了运行一行命令重置 Ubuntu，几分钟就能复活。
   * **需要注意的风险：** 因为它能通过 `/mnt/c/` 访问你的 Windows 文件，所以**不要**让不受信任的 Agent 随意执行涉及大范围删除文件的高危指令。

   总而言之，它拥有完成绝大多数日常自动化、编程、文件处理任务的所有正常权限，是一个非常完美且强大的运行状态！


## 代理

在默认的 **NAT 模式**下，WSL2 就像是一台通过“虚拟路由器”连在你 Windows 电脑上的**另一台独立电脑**。
* 当你在 Windows 的代理软件里设置监听 `localhost`（即 `127.0.0.1`）时，它只允许 Windows 本机访问。
* 当你在 WSL (Ubuntu) 里面访问 `localhost` 时，它指向的是 **Ubuntu 自己**，而不是你的 Windows。
* 所以，Ubuntu 找不到 Windows 上的代理，导致你接下来运行 OpenClaw 安装脚本（需要从 GitHub 下载东西）时，大概率会遇到**网络超时、下载失败**的问题。


请按照以下步骤操作，一劳永逸地解决 WSL 的网络和代理问题：

- 第一步：检查你的代理软件设置
   打开你 Windows 上的代理软件（Clash / v2ray 等）：
   1. 确保开启了 **TUN 模式**（如果有的话，开启 TUN 模式通常能全局接管 WSL 流量）。
   2. 或者开启 **“允许局域网连接” (Allow LAN)**。

-  第二步：创建 `.wslconfig` 配置文件
   我们需要告诉 Windows，让 WSL 使用全新的镜像网络模式。

   1. 按下 `Win + R` 键，打开“运行”窗口。
   2. 输入 `%USERPROFILE%` 并回车，这会打开你的 Windows 用户文件夹（例如 `C:\Users\你的用户名`）。
   3. 在这个文件夹里，右键 -> 新建 -> **文本文档**。
   4. 将这个文档重命名为 `.wslconfig` （注意：**前面有一个点，且没有 .txt 后缀**。系统如果提示改变扩展名，选“是”）。

- 第三步：写入配置代码
   用记事本打开刚刚创建的 `.wslconfig` 文件，把下面这两段代码复制进去并保存：

   ```ini
   [wsl2]
   networkingMode=mirrored

   [experimental]
   autoMemoryReclaim=dropcache
   hostAddressLoopback=true
   ```
   > **参数解释：**
   > * `networkingMode=mirrored`：让 WSL 共享 Windows 的网络、IP 和代理。
   > * `hostAddressLoopback=true`：允许 WSL 直接通过 `localhost` 访问 Windows 上的代理端口。
   > * `autoMemoryReclaim`：顺手帮你解决 WSL 吃内存的问题（空闲时自动释放内存）。

- 第四步：重启 WSL 使配置生效
  1. 再次打开 PowerShell（以管理员身份）。
  2. 输入以下命令彻底关闭当前运行的 WSL：
     ```powershell
     wsl --shutdown
     ```
  3. 重新在开始菜单打开你的 **Ubuntu** 终端。


- 验证是否成功

   现在，你的 WSL 应该已经可以直接使用 Windows 的代理了！如果你想测试一下在 Ubuntu 里能否连通外网，可以在 Ubuntu 的黑框里输入：

   ```bash
   curl -I https://google.com
   ```

   如果有类似 `HTTP/2 200` 或者返回了网页代码，说明网络已经完全打通。


## 手动安装 Node.js（绕开拦截，100% 成功率）

既然 `apt` 包管理器和代理软件“八字不合”，那我们就**不用它了**。
OpenClaw 脚本其实非常聪明：**如果它检测到系统里已经安装了 Node.js（22 及以上版本），它就会直接跳过下载步骤。**

我们可以使用 Node.js 官方推荐的 `nvm` 工具来手动安装，这种方式极少受到代理软件 Fake IP 的干扰。

**1. 安装 NVM (Node Version Manager)**
在 Ubuntu 终端复制粘贴并执行：
```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
```

**2. 刷新环境变量（让 NVM 生效）**
```bash
source ~/.bashrc
```

**3. 安装 Node.js 22 版本**
```bash
nvm install 22
```
*(这一步会从 Node.js 官方服务器直接拉取压缩包，通常你的代理软件能完美加速这个过程。)*

**4. 验证是否安装成功**
输入 `node -v`，如果返回 `v22.x.x`，说明 Node.js 已经完美就绪！

**5. 重新运行 OpenClaw 脚本**
现在，你可以再次运行 OpenClaw 的一键安装脚本了：
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```
这次脚本检测到 Node.js 已经存在，就会直接跳过刚才报错的那一步，顺利完成后续的安装。







## 在线搜索

在 https://searx.space/ 中寻找开源聚合搜索

在 ~/.openclaw/.env 文件中添加：

```
# 指定搜索提供商为 searxng
SEARXNG_BASE_URL=https://search.rhscz.eu
# 确保输出格式为 json (OpenClaw 通常会自动处理，但建议显式指定)
SEARXNG_FORMAT=json

```

## 模型

在 ~/.openclaw/.env 文件中添加：

```
# 设置提供商为 google
LLM_PROVIDER=google

# 填入刚刚获取的 API Key
GOOGLE_API_KEY=AIzaSy...your_key_here...

# 指定模型名称 (根据 2026 年最新型号选择)
# 推荐使用 flash 版本以节省 Token 并获得极速响应
DEFAULT_LLM_MODEL=gemini-2.0-flash 
# 或者使用经典的 gemini-1.5-flash
```
