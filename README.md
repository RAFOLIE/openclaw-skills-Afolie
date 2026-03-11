# OpenClaw Skills - Afolie

OpenClaw 自定义技能备份仓库

## 📦 技能列表

### 1. force-tool-first 🔧

**版本**: 1.5

强制优先调用本地工具检查环境的技能（支持自学习优化）。

**核心功能**:
- 工具优先法则（永不猜测，永远执行工具）
- 自学习迭代（成功案例自动记录）
- Windows 平台适配（PowerShell 命令）
- 软件安装支持（winget、环境变量刷新）
- pty 交互式命令处理
- **必须使用专用 CLI**（Obsidian → obsidian-cli，GitHub → gh，浏览器 → pinchtab）

### 2. obsidian-cli 💎

**版本**: 1.0

Obsidian 笔记操作技能 - 必须通过 obsidian-cli 完成。

**功能**:
- 搜索笔记（按名称或内容）
- 创建笔记
- 移动/重命名笔记（自动更新链接）
- 删除笔记

### 3. pinchtab 🌐

**版本**: 1.0 | **Stars**: 6,474

高性能浏览器自动化工具，给 AI Agent 使用的浏览器控制。

**功能**:
- 浏览器自动化（导航、点击、截图、提取文本）
- Token 高效（文本提取比截图便宜 5-13x）
- 多实例浏览器管理
- HTTP API (端口 9867)

**快速命令**:
```bash
pinchtab                    # 启动服务
pinchtab nav <url>          # 导航
pinchtab snap -i -c         # 获取页面结构
pinchtab click <ref>        # 点击元素
pinchtab text               # 提取文本
```

### 4. skill-creator 🎨

创建、修改和优化 OpenClaw 技能。

**功能**:
- 创建新技能
- 修改现有技能
- 测试技能性能
- 优化技能描述

### 5. skill-vetter 🔒

**版本**: 1.0

安全审查技能 - 在安装任何技能前先检查安全性。

**功能**:
- 源头检查（作者、下载量、更新时间）
- 代码审查（检测恶意代码）
- 权限评估（文件、网络、命令）
- 风险分级（LOW/MEDIUM/HIGH/EXTREME）

**口诀**: *Paranoia is a feature.* 🔒

---

## 📥 安装方法

将技能文件夹复制到 OpenClaw workspace 目录：

```bash
# Windows
cp -r * "$env:USERPROFILE\.openclaw\workspace\skills\"

# macOS/Linux
cp -r * ~/.openclaw/workspace/skills/
```

## 🔧 依赖工具

| 工具 | 用途 |
|------|------|
| `gh` | GitHub CLI 操作 |
| `obsidian-cli` | Obsidian 笔记操作 |
| `pinchtab` | 浏览器自动化 |

## 📝 版本历史

- **2026-03-11** - 添加 pinchtab、skill-creator、skill-vetter
- **2026-03-11** - 创建仓库，上传 force-tool-first v1.5 + obsidian-cli v1.0

## 👤 作者

小企鹅 🐧

## 📄 License

MIT
