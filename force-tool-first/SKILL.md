---
name: force-tool-first
description: |
  强制优先调用本地工具检查环境的技能（支持自学习优化）。
  
  **当以下情况必须使用**：
  - 用户询问 git 仓库、分支、提交、状态、pull/push、远程项目
  - 用户要求检查环境、目录、文件、进程、端口
  - 用户提到运行状态、服务启动/停止
  - 用户要求安装软件、工具、CLI
  - 用户说"帮我看看"、"检查一下"、"是什么"、"帮我安装"
  - 涉及系统配置、路径、网络连接
  
  **自学习机制**：成功解决的问题会自动记录到 `examples/` 目录，
  下次遇到类似问题优先参考已验证的解决方案。

version: 1.6
user-invocable: true
metadata:
  openclaw:
    emoji: "🔧"
    priority: high
    force-tool-first: true
    self-improving: true
    examples-dir: "skills/force-tool-first/examples"
---

# Force Tool First - 工具优先法则（自学习版）

## 核心铁律（不可违反）

1. **永远先调用工具**
   - 当用户问题可以通过工具得到确切答案时，**必须**先调用工具
   - 绝对禁止在没有工具结果的情况下猜测或说"我没有能力"

2. **常用工具优先级**
   - `exec` 工具：执行 shell 命令（git、文件操作、系统检查、软件安装）
   - `read` 工具：读取文件内容
   - `process` 工具：管理后台进程、pty 交互
   - `write` 工具：创建或覆盖文件
   - `edit` 工具：精确编辑文件

3. **⚠️ 必须使用专用 CLI（禁止直接操作）**
   - **Obsidian 笔记**：必须使用 `obsidian-cli` 技能，禁止直接 `exec` 操作 .md 文件
   - **GitHub 操作**：必须使用 `gh` CLI / `github` 技能，禁止直接调用 GitHub API
   - **原因**：
     - Obsidian：保持链接完整、触发插件、维护关系图
     - GitHub：认证正确、API 规范、避免限流

4. **🌐 浏览器使用优先级**
   - **上网/网页操作**：优先使用 **PinchTab**（token 高效，文本提取）
   - **何时用 OpenClaw browser**：需要截图、复杂交互、或 PinchTab 不可用时
   - **PinchTab 配置**：`~/.openclaw/workspace/skills/pinchtab/SKILL.md`
   - **服务地址**：http://localhost:9867
   - **优势**：
     - Token 消耗比截图便宜 5-13x
     - 原生多实例和 profile 隔离
     - 更适合自动化任务

5. **平台适配**
   - Windows: 优先使用 PowerShell 语法
   - 注意路径分隔符（`\` vs `/`）
   - Windows 包管理器：`winget`、`choco`、`scoop`

## 重要技巧

### 🔄 新安装工具后刷新环境变量
刚安装完软件后，当前 shell 的 PATH 不会自动更新。需要刷新：

```powershell
# 方法一：重新加载环境变量后执行
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User"); <新命令>

# 方法二：使用完整路径
& "C:\Program Files\GitHub CLI\gh.exe" --version
```

### 🖥️ 交互式命令处理
需要用户交互的命令（如登录流程）使用 `pty: true`：

```json
{
  "tool": "exec",
  "command": "gh auth login",
  "pty": true,
  "timeout": 180
}
```

然后用 `process` 工具发送按键：
- `process(action=send-keys, keys=["enter"])`
- `process(action=poll, timeout=5000)` 查看输出

### 📦 Windows 软件安装
```powershell
# 使用 winget 安装（推荐）
winget install <包名> --accept-source-agreements --accept-package-agreements

# 使用 choco 安装
choco install <包名> -y

# 使用 scoop 安装
scoop install <包名>
```

### 🌐 PinchTab 服务管理（Windows）

**⚠️ PinchTab 服务稳定性问题**：
- 服务可能意外退出（进程 code=1）
- 需要更健壮的重启机制

**推荐做法**：
1. **使用长时间 timeout**：600-900 秒
2. **操作前检查服务**：
   ```powershell
   if (-not (Test-NetConnection localhost 9867 -InformationLevel Quiet)) {
       # 重启服务
       node "$env:APPDATA\npm\node_modules\pinchtab\bin\pinchtab" --port 9867
       Start-Sleep -Seconds 5
   }
   ```
3. **不要自动关闭浏览器**（用户可能正在登录）
4. **Twitter 查看使用"热门"**（`f=top`）而非"最新"

**服务启动示例**：
```powershell
# 后台启动服务（长时间运行）
node "$env:APPDATA\npm\node_modules\pinchtab\bin\pinchtab" --port 9867
# 参数：background=true, timeout=900

# 创建实例
$body = @{name='twitter'; mode='headed'} | ConvertTo-Json
Invoke-RestMethod -Uri 'http://localhost:9867/instances/launch' -Method POST -Body $body -ContentType 'application/json'
```

### 📥 GitHub 仓库克隆默认路径
用户偏好：GitHub 仓库克隆到 `用户文件夹/Documents/GitHub/` 目录

**⚠️ 克隆前先确认路径**：
```
用户请求克隆 → 告知默认路径 → 用户确认/指定 → 执行克隆
```

```powershell
# 默认克隆路径（Windows）
$githubPath = "$env:USERPROFILE\Documents\GitHub"

# 确保目录存在
if (-not (Test-Path $githubPath)) { New-Item -ItemType Directory -Path $githubPath -Force }

# 克隆到默认路径
gh repo clone <用户名>/<仓库名> "$githubPath\<仓库名>"
# 或
git clone https://github.com/<用户名>/<仓库名>.git "$githubPath\<仓库名>"
```

## 自学习迭代机制

### 触发条件
当满足以下条件时，自动记录成功案例：
- 用户问题得到解决
- 用户明确表示满意（"好了"、"解决了"、"完美"、"弄好了"等）
- 使用的工具调用具有复用价值

### 记录流程

```
问题解决成功
    ↓
判断是否有复用价值
    ↓
[有] 提取关键信息：
    - 问题类型/关键词
    - 使用的工具和命令
    - 完整的调用参数
    - 成功的输出结果示例
    ↓
写入 examples/<问题类型>.md
    ↓
更新 SKILL.md 的触发场景
```

### 示例文件格式

每个成功案例保存为 `examples/<类型>.md`：

```markdown
# 示例：检查 OpenClaw Gateway 状态

## 触发关键词
- gateway 状态
- openclaw 服务
- 检查服务是否运行

## 问题描述
用户想检查 OpenClaw Gateway 是否正常运行

## 成功的工具调用

### 方法一：检查进程
\`\`\`json
{
  "tool": "exec",
  "command": "Get-Process | Where-Object {$_.ProcessName -like '*openclaw*'}",
  "platform": "windows"
}
\`\`\`

## 成功输出示例
（实际输出）

## 记录时间
2026-03-11

## 效果
用户确认 Gateway 正在运行，问题解决
```

### 示例库结构

```
skills/force-tool-first/
├── SKILL.md
├── examples/
│   ├── git-status-check.md
│   ├── github-cli-login.md
│   ├── software-install.md
│   ├── gateway-health-check.md
│   └── file-list.md
└── learnings/
    └── success-patterns.md  # 聚合的高频成功模式
```

## 触发场景清单

### Git & GitHub 相关
- "看看仓库"、"git status"、"有什么改动"
- "当前分支"、"需要pull吗"、"commit情况"
- "远程仓库"、"同步"、"推送"
- "我的 GitHub 项目"、"线上仓库"
- "GitHub 仓库列表"

### 软件安装
- "帮我安装"、"安装一下"
- "有没有 xxx 工具"
- "装个 gh/git/node"

### 环境检查
- "当前目录"、"在哪"、"工作目录"
- "有什么文件"、"文件夹内容"
- "环境变量"、"配置在哪"
- "服务状态"、"gateway"、"openclaw"

### 系统状态
- "运行状态"、"进程"、"是否启动"
- "端口占用"、"连接情况"
- "系统信息"、"版本"

### 文件操作
- "读取这个文件"、"看看内容"
- "查找文件"、"是否存在"

### Obsidian 笔记
- "搜索笔记"、"找笔记"
- "创建笔记"、"新建笔记"
- "Obsidian"、"笔记仓库"
- "移动笔记"、"重命名笔记"

## 执行流程

```
用户提问
    ↓
首先检查 examples/ 目录
是否有类似问题的成功案例？
    ↓
[有] → 优先使用已验证的方法
[无] → 选择合适的工具尝试
    ↓
执行并获取结果
    ↓
问题解决？
    ↓
[是] → 询问用户是否满意
    ↓
[满意] → 自动记录到 examples/
    ↓
基于结果回答用户
```

## 错误处理

1. **工具执行失败**
   - 说明失败原因
   - 提供可能的解决方案
   - 不要假装成功

2. **命令不存在**
   - 检查是否刚安装（需要刷新环境变量）
   - 检查是否拼写错误
   - 建议安装或替代方案

3. **权限问题**
   - 明确告知需要什么权限
   - 建议用户如何处理

4. **交互式命令超时**
   - 使用 `pty: true` 启动
   - 用 `process` 工具保持会话
   - 告诉用户需要等待的操作

## 回复风格

**正确示例**：
```
用户：当前目录有什么文件？
→ [先查 examples/，发现 file-list.md]
→ [使用已验证方法: Get-ChildItem]
→ 回复：当前目录有 5 个文件：README.md、package.json、...
```

**错误示例**：
```
用户：当前目录有什么文件？
→ ❌ "我无法直接访问文件系统"（错误！应该用工具）
```

## Windows 常用命令速查

| 场景 | 命令 |
|------|------|
| 当前目录 | `Get-Location` 或 `pwd` |
| 列出文件 | `Get-ChildItem` 或 `ls` |
| Git 状态 | `git status` |
| Git 分支 | `git branch` |
| GitHub 仓库列表 | `gh repo list <用户名> --limit 100` |
| GitHub 克隆 | `gh repo clone <用户名>/<仓库名> "$env:USERPROFILE\Documents\GitHub\<仓库名>"` |
| 进程检查 | `Get-Process | Where-Object {$_.ProcessName -like "*关键词*"}` |
| 端口检查 | `netstat -ano | findstr "端口号"` |
| 文件内容 | `Get-Content <文件名>` |
| 安装软件 | `winget install <包名> --accept-source-agreements --accept-package-agreements` |
| 刷新 PATH | `$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")` |

### 🌐 PinchTab 浏览器自动化（AI 专用）

**版本**：0.7.8 | **端口**：9867 | **Stars**：6,474

PinchTab 是给 AI Agent 使用的浏览器控制工具，Token 消耗比截图便宜 5-13x。

| 场景 | 命令 |
|------|------|
| 启动服务 | `pinchtab` |
| 导航到 URL | `pinchtab nav https://example.com` |
| 获取页面结构 | `pinchtab snap -i -c` |
| 点击元素 | `pinchtab click e5` |
| 提取文本 | `pinchtab text` |
| 查看帮助 | `pinchtab --help` |

**HTTP API**（端口 9867）：
```bash
# 创建实例
curl -X POST http://localhost:9867/instances/launch -d '{"name":"work","mode":"headless"}'

# 打开标签
curl -X POST http://localhost:9867/instances/<INST>/tabs/open -d '{"url":"https://..."}'

# 获取快照
curl "http://localhost:9867/tabs/<TAB>/snapshot?filter=interactive"

# 点击元素
curl -X POST http://localhost:9867/tabs/<TAB>/action -d '{"kind":"click","ref":"e5"}'
```

**何时使用**：
- 需要浏览器自动化（导航、点击、截图、提取文本）
- 需要高效 Token 消耗（文本提取 > 截图）
- 需要多实例浏览器管理
- 用户提到"浏览器自动化"、"控制 Chrome"

---

### 💎 Obsidian CLI 常用命令

**⚠️ 重要：所有 Obsidian 操作必须通过 obsidian-cli 完成，禁止直接修改文件！**

| 场景 | 命令 |
|------|------|
| 设置默认仓库 | `obsidian-cli set-default "<仓库名>"` |
| 查看默认仓库 | `obsidian-cli print-default` |
| 查看默认仓库路径 | `obsidian-cli print-default --path-only` |
| 搜索笔记（按名称） | `obsidian-cli search "关键词"` |
| 搜索笔记（内容） | `obsidian-cli search-content "关键词"` |
| 创建笔记 | `obsidian-cli create "文件夹/笔记名" --content "内容" --open` |
| 移动/重命名 | `obsidian-cli move "旧路径/笔记" "新路径/笔记"` |
| 删除笔记 | `obsidian-cli delete "路径/笔记"` |

**为什么必须用 CLI**：
- 自动更新 `[[wikilinks]]` 链接
- 保持 Obsidian 元数据完整
- 触发插件和同步
- 维护笔记关系图

## 记住

- **工具优先，永远不要猜**
- **结果说话，不要凭空说**
- **成功了要记录，经验要积累**
- **失败了就诚实说明，不要装作成功**
- **新装工具记得刷新环境变量**
