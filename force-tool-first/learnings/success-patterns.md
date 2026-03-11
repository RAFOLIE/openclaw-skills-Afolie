# Success Patterns - 高频成功模式汇总

这个文件聚合了 `examples/` 目录中的高频成功模式，用于快速参考。

## 分类索引

### 1. 服务状态检查
- [gateway-health-check.md](../examples/gateway-health-check.md)
- 常用命令：`Get-Process`, `netstat`, `openclaw gateway status`
- 关键指标：进程存在、端口监听

### 2. Git & GitHub 操作
- [git-status-check.md](../examples/git-status-check.md) - 本地仓库状态
- [github-cli-list-repos.md](../examples/github-cli-list-repos.md) - GitHub 远程仓库
- 常用命令：`git status`, `gh repo list`, `gh auth login`
- 关键技巧：pty 交互式登录

### 3. 文件系统
- [file-list.md](../examples/file-list.md)
- 常用命令：`Get-ChildItem`, `Get-Content`
- 关键指标：文件名、大小、修改时间

### 4. 软件安装
- [software-install-winget.md](../examples/software-install-winget.md)
- 常用命令：`winget install`, `winget search`
- 关键技巧：安装后刷新 PATH

### 5. Obsidian 笔记
- 常用命令：`obsidian-cli search`, `obsidian-cli create`, `obsidian-cli move`
- 关键技巧：先设置默认仓库

## 通用模式

### Windows PowerShell 常用命令

| 场景 | 命令 |
|------|------|
| 当前目录 | `Get-Location` |
| 列出文件 | `Get-ChildItem` |
| 查看文件 | `Get-Content <文件名>` |
| 查找进程 | `Get-Process | Where-Object {$_.ProcessName -like "*关键词*"}` |
| 检查端口 | `netstat -ano | findstr "端口号"` |
| 安装软件 | `winget install <包名> --accept-source-agreements --accept-package-agreements` |
| 刷新 PATH | `$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")` |

### 通用 Git 命令

| 场景 | 命令 |
|------|------|
| 状态 | `git status` |
| 分支 | `git branch` |
| 日志 | `git log --oneline -10` |
| 远程 | `git remote -v` |

### GitHub CLI 命令

| 场景 | 命令 |
|------|------|
| 登录 | `gh auth login` |
| 状态 | `gh auth status` |
| 仓库列表 | `gh repo list <用户名> --limit 100` |
| 仓库详情 | `gh repo view <用户名>/<仓库名>` |
| 克隆仓库 | `gh repo clone <用户名>/<仓库名> "$env:USERPROFILE\Documents\GitHub\<仓库名>"` |

### 💎 Obsidian CLI 命令

| 场景 | 命令 |
|------|------|
| 设置默认仓库 | `obsidian-cli set-default "<仓库名>"` |
| 查看默认仓库 | `obsidian-cli print-default` |
| 搜索笔记（名称） | `obsidian-cli search "关键词"` |
| 搜索笔记（内容） | `obsidian-cli search-content "关键词"` |
| 创建笔记 | `obsidian-cli create "文件夹/笔记名" --content "内容"` |
| 移动/重命名 | `obsidian-cli move "旧路径" "新路径"` |
| 删除笔记 | `obsidian-cli delete "路径/笔记"` |

### 📥 GitHub 默认克隆路径

用户偏好：`用户文件夹/Documents/GitHub/`
```
Windows: $env:USERPROFILE\Documents\GitHub\
macOS: ~/Documents/GitHub/
Linux: ~/Documents/GitHub/
```

## 重要技巧

### 🔄 新安装工具后刷新 PATH
```powershell
$env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
```

### 🖥️ 交互式命令
需要用户交互的命令使用 pty 模式：
```json
{
  "tool": "exec",
  "command": "<交互式命令>",
  "pty": true,
  "timeout": 180
}
```

然后用 process 工具交互：
- `process(action=send-keys, keys=["enter"])`
- `process(action=poll, timeout=5000)`

## 统计

- 示例总数：5
- 分类数量：5（服务检查、Git/GitHub、文件系统、软件安装、Obsidian）
- 最后更新：2026-03-11
- 创建者：小企鹅 🐧

---

*此文件会在新增示例时自动更新*
