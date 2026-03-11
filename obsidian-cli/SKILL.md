---
name: obsidian-cli
description: |
  Obsidian 笔记操作技能 - 必须通过 obsidian-cli 完成。
  
  **核心原则**：所有对 Obsidian 笔记的操作必须使用 obsidian-cli，禁止直接修改文件。
  
  **当以下情况必须使用**：
  - 搜索、创建、移动、删除 Obsidian 笔记
  - 用户提到"笔记"、"Obsidian"、"仓库"
  
  **为什么必须用 obsidian-cli**：
  - 自动更新 [[wikilinks]] 链接
  - 保持 Obsidian 元数据完整
  - 触发插件和同步
  - 维护笔记关系图

version: 1.0
metadata:
  openclaw:
    emoji: "💎"
    requires:
      bins: ["obsidian-cli"]
---

# Obsidian CLI 技能

## 核心原则

**⚠️ 禁止直接修改文件！**

所有 Obsidian 笔记操作必须通过 `obsidian-cli` 完成，原因：
1. **链接自动更新** - 移动笔记时自动更新所有 `[[wikilinks]]`
2. **元数据完整** - 保持 frontmatter 和标签结构
3. **插件触发** - 让相关插件感知变更
4. **同步正常** - 确保云同步正确识别改动

## 前置条件

### 检查 obsidian-cli
```bash
obsidian-cli --version
```

### 安装 obsidian-cli（如果未安装）
```bash
# macOS
brew install yakitrak/yakitrak/obsidian-cli

# Windows (npm)
npm install -g obsidian-cli
```

### 设置默认仓库
```bash
# 查看可用仓库
obsidian-cli list-vaults

# 设置默认仓库
obsidian-cli set-default "<仓库名>"
```

## 命令参考

### 仓库管理

```bash
# 列出所有仓库
obsidian-cli list-vaults

# 设置默认仓库
obsidian-cli set-default "<仓库名>"

# 查看默认仓库名
obsidian-cli print-default

# 查看默认仓库路径
obsidian-cli print-default --path-only
```

### 搜索笔记

```bash
# 按名称搜索
obsidian-cli search "关键词"

# 搜索内容（显示片段和行号）
obsidian-cli search-content "关键词"

# 搜索结果示例：
# Notes/项目笔记.md:42: 这里的内容匹配了关键词
```

### 创建笔记

```bash
# 创建笔记
obsidian-cli create "文件夹/笔记名" --content "笔记内容"

# 创建并打开
obsidian-cli create "文件夹/笔记名" --content "内容" --open

# 创建到根目录
obsidian-cli create "新笔记" --content "内容"
```

**注意**：
- 需要 Obsidian URI handler (`obsidian://`) 正常工作
- 不要在隐藏文件夹（如 `.obsidian/`）下创建笔记

### 移动/重命名笔记

```bash
# 移动笔记
obsidian-cli move "旧文件夹/笔记" "新文件夹/笔记"

# 重命名笔记
obsidian-cli move "旧名字" "新名字"

# 移动并重命名
obsidian-cli move "旧文件夹/旧名" "新文件夹/新名"
```

**重要**：这是 obsidian-cli 的核心价值！
- ✅ 自动更新所有 `[[wikilinks]]` 引用
- ✅ 自动更新 Markdown 链接
- ✅ 保持笔记关系图正确
- ❌ 如果用 `mv` 或直接改文件，链接会断开！

### 删除笔记

```bash
# 删除笔记
obsidian-cli delete "文件夹/笔记名"
```

## 工作流程示例

### 创建新笔记
```
1. obsidian-cli create "项目/2026-03-11 会议记录" --content "# 会议记录\n\n日期：2026-03-11\n\n## 议题\n\n"
2. 返回：笔记创建成功
```

### 搜索并移动笔记
```
1. obsidian-cli search "会议"
2. 找到：会议记录.md
3. obsidian-cli move "会议记录.md" "归档/2026-03/会议记录.md"
4. 返回：移动成功，所有链接已更新
```

### 批量整理
```
1. obsidian-cli search-content "待办"
2. 找到多个包含"待办"的笔记
3. 逐个移动：
   obsidian-cli move "笔记1" "待办/笔记1"
   obsidian-cli move "笔记2" "待办/笔记2"
```

## 错误处理

### "obsidian-cli not found"
- 原因：未安装 obsidian-cli
- 解决：`npm install -g obsidian-cli` 或 `brew install yakitrak/yakitrak/obsidian-cli`

### "No default vault set"
- 原因：未设置默认仓库
- 解决：`obsidian-cli set-default "<仓库名>"`

### "Obsidian URI handler not working"
- 原因：Obsidian 未正确注册 URI 协议
- 解决：确保 Obsidian 已安装并运行过一次

### "Note not found"
- 原因：路径错误或笔记不存在
- 解决：使用 `search` 确认正确路径

## 配置文件位置

Obsidian 仓库配置文件：
- **macOS**: `~/Library/Application Support/obsidian/obsidian.json`
- **Windows**: `%APPDATA%\obsidian\obsidian.json`
- **Linux**: `~/.config/obsidian/obsidian.json`

## 记住

- ✅ **永远用 obsidian-cli 操作笔记**
- ✅ **移动笔记时链接自动更新是核心价值**
- ❌ **禁止直接编辑 .md 文件**
- ❌ **禁止用 mv/cp 命令移动笔记**
- 💎 **Obsidian 的价值在于链接和关系图，粗暴操作会破坏它们**
