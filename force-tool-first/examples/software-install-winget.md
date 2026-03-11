# 示例：安装软件（Windows winget）

## 触发关键词
- 帮我安装
- 安装一下
- 装个 xxx
- 有没有 xxx 工具
- 需要安装

## 问题描述
用户想在 Windows 上安装某个软件或工具

## 成功的工具调用

### 方法一：使用 winget 安装（推荐）
```json
{
  "tool": "exec",
  "command": "winget install <包名> --accept-source-agreements --accept-package-agreements",
  "platform": "windows",
  "timeout": 120
}
```

**常用包名**：
| 软件 | 包名 |
|------|------|
| GitHub CLI | `GitHub.cli` |
| Node.js | `OpenJS.NodeJS.LTS` |
| Python | `Python.Python.3.12` |
| Git | `Git.Git` |
| VS Code | `Microsoft.VisualStudioCode` |
| 7-Zip | `7zip.7zip` |

### 方法二：搜索可用包
```json
{
  "tool": "exec",
  "command": "winget search <关键词>",
  "platform": "windows"
}
```

### 方法三：检查是否已安装
```json
{
  "tool": "exec",
  "command": "winget list <包名>",
  "platform": "windows"
}
```

### 方法四：安装后验证（需刷新 PATH）
```json
{
  "tool": "exec",
  "command": "$env:Path = [System.Environment]::GetEnvironmentVariable(\"Path\",\"Machine\") + \";\" + [System.Environment]::GetEnvironmentVariable(\"Path\",\"User\"); <命令> --version",
  "platform": "windows"
}
```

## 成功输出示例

**安装成功**：
```
Found GitHub CLI [GitHub.cli] Version 2.88.0
Downloading https://github.com/cli/cli/releases/download/v2.88.0/gh_2.88.0_windows_amd64.msi
Successfully verified installer hash
Starting package install...
Successfully installed
```

**验证成功**：
```
gh version 2.88.0 (2026-03-10)
https://github.com/cli/cli/releases/tag/v2.88.0
```

## 记录时间
2026-03-11

## 效果
成功安装了 GitHub CLI，用户可以使用 gh 命令

## 关键技巧

1. **自动接受协议**：
   - `--accept-source-agreements`：接受源协议
   - `--accept-package-agreements`：接受包协议
   - 避免交互式确认

2. **超时设置**：安装可能需要时间，设置 `timeout: 120` 或更长

3. **安装后立即使用**：需要刷新 PATH 环境变量
   ```powershell
   $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
   ```

4. **备选方案**：
   - `choco install <包名> -y`
   - `scoop install <包名>`
