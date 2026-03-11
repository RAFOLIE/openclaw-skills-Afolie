# 示例：列出 GitHub 远程仓库

## 触发关键词
- GitHub 仓库列表
- 我的 GitHub 项目
- 线上仓库
- 远程仓库
- GitHub 有哪些项目

## 问题描述
用户想查看自己 GitHub 账号下的所有远程仓库

## 前置条件
- 已安装 GitHub CLI (`gh`)
- 已登录 GitHub 账号

### 检查是否安装
```json
{
  "tool": "exec",
  "command": "gh --version",
  "platform": "any"
}
```

### 安装 GitHub CLI（如果未安装）
```json
{
  "tool": "exec",
  "command": "winget install GitHub.cli --accept-source-agreements --accept-package-agreements",
  "platform": "windows",
  "timeout": 120
}
```

### 登录 GitHub CLI（交互式）
```json
{
  "tool": "exec",
  "command": "gh auth login",
  "pty": true,
  "timeout": 180
}
```

登录流程：
1. 选择 GitHub.com
2. 选择 HTTPS
3. 选择 Login with a web browser
4. 复制显示的一次性验证码
5. 打开 https://github.com/login/device
6. 输入验证码授权
7. 按 Enter 完成

## 成功的工具调用

### 方法一：列出仓库（表格格式）
```json
{
  "tool": "exec",
  "command": "$env:Path = [System.Environment]::GetEnvironmentVariable(\"Path\",\"Machine\") + \";\" + [System.Environment]::GetEnvironmentVariable(\"Path\",\"User\"); gh repo list <用户名> --limit 100 --json name,url,description,isPrivate,updatedAt,stargazerCount | ConvertFrom-Json | Format-Table name, isPrivate, stargazerCount -AutoSize",
  "platform": "windows"
}
```

### 方法二：获取完整 JSON
```json
{
  "tool": "exec",
  "command": "$env:Path = [System.Environment]::GetEnvironmentVariable(\"Path\",\"Machine\") + \";\" + [System.Environment]::GetEnvironmentVariable(\"Path\",\"User\"); gh repo list <用户名> --limit 100 --json name,url,description,isPrivate,updatedAt,stargazerCount",
  "platform": "windows"
}
```

### 方法三：检查登录状态
```json
{
  "tool": "exec",
  "command": "$env:Path = [System.Environment]::GetEnvironmentVariable(\"Path\",\"Machine\") + \";\" + [System.Environment]::GetEnvironmentVariable(\"Path\",\"User\"); gh auth status",
  "platform": "windows"
}
```

## 成功输出示例

**表格格式**：
```
name                     isPrivate stargazerCount
----                     --------- --------------
example-repo-1               False              2
example-repo-2               False              0
example-private-repo          True              0
example-repo-3               False              0
```

**JSON 格式**：
```json
[
  {
    "name": "example-repo-1",
    "url": "https://github.com/<用户名>/example-repo-1",
    "description": "项目描述...",
    "isPrivate": false,
    "stargazerCount": 2,
    "updatedAt": "2026-02-27T09:48:17Z"
  }
]
```

## 记录时间
2026-03-11

## 效果
成功列出了用户的所有 GitHub 仓库，包括公开和私有项目

## 关键技巧

1. **新安装后刷新 PATH**：
   ```powershell
   $env:Path = [System.Environment]::GetEnvironmentVariable("Path","Machine") + ";" + [System.Environment]::GetEnvironmentVariable("Path","User")
   ```

2. **pty 交互**：登录流程需要用户在浏览器中操作，使用 pty 模式保持会话

3. **JSON 输出更详细**：使用 `--json` 可以获取更丰富的字段信息

## 关联操作：克隆仓库

用户偏好：GitHub 仓库默认克隆到 `用户文件夹/Documents/GitHub/` 目录

### ⚠️ 克隆前先确认路径

**推荐流程**：先询问用户确认安装位置，再执行克隆

```
用户说：帮我克隆 xxx 仓库
    ↓
回复示例：
"咕嘎！要克隆到默认路径吗？
📁 默认：$env:USERPROFILE\Documents\GitHub\xxx
或者告诉我你想放到哪里？"
    ↓
[用户确认] → 执行克隆
[用户指定其他路径] → 使用指定路径
```

### 克隆命令

```powershell
# 设置默认克隆路径
$githubPath = "$env:USERPROFILE\Documents\GitHub"

# 确保目录存在
if (-not (Test-Path $githubPath)) { New-Item -ItemType Directory -Path $githubPath -Force }

# 克隆到默认路径
gh repo clone <用户名>/<仓库名> "$githubPath\<仓库名>"
```

**示例**：
```powershell
# 克隆示例仓库到默认路径
$githubPath = "$env:USERPROFILE\Documents\GitHub"
gh repo clone <用户名>/<仓库名> "$githubPath\<仓库名>"
```

### 完整交互示例

**用户**：帮我克隆 example-repo 仓库

**小企鹅**：
```
咕嘎！要克隆 <用户名>/example-repo 仓库

📁 默认路径：$env:USERPROFILE\Documents\GitHub\example-repo
📂 或者告诉我你想放到哪里？

确认默认路径？(是/否)
```

**用户**：是

**小企鹅**：[执行克隆命令]
