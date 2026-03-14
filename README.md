# OpenClaw Skills - Afolie

OpenClaw 自定义技能备份仓库

## 📦 技能列表

### 1. force-tool-first 🔧

**版本**: 1.6

强制优先调用本地工具检查环境的技能（支持自学习优化）。

**核心功能**:
- 工具优先法则（永不猜测，永远执行工具）
- 自学习迭代（成功案例自动记录）
- Windows 平台适配（PowerShell 命令）
- 软件安装支持（winget、环境变量刷新）
- pty 交互式命令处理
- **浏览器操作使用内置 browser 工具**

### 2. obsidian-cli 💎

**版本**: 1.0

Obsidian 笔记操作技能 - 必须通过 obsidian 命令完成。

**功能**:
- 搜索笔记（按名称或内容）
- 创建笔记
- 移动/重命名笔记（自动更新链接）
- 删除笔记

### 3. skill-creator 🎨

**版本**: 1.1

创建、修改和优化 OpenClaw 技能。

**功能**:
- 创建新技能
- 修改现有技能
- 从对话历史提取工作流创建技能
- 测试技能性能
- 优化技能描述

### 4. skill-vetter 🔒

**版本**: 1.0

安全审查技能 - 在安装任何技能前先检查安全性。

**功能**:
- 源头检查（作者、下载量、更新时间）
- 代码审查（检测恶意代码）
- 权限评估（文件、网络、命令）
- 风险分级（LOW/MEDIUM/HIGH/EXTREME）

**口诀**: *Paranoia is a feature.* 🔒

### 5. twitter-trending 🐦

**版本**: 1.6

Twitter/X 推文浏览与深度解读技能。

**功能**:
- 🏠 「为你推荐」首页浏览（默认，利用登录态）
- 🔍 关键词搜索 / 🕐 最新推文
- 用户 Chrome 浏览器直连（profile=user，已登录态）
- 固定 10 条 + 每条深度解读 + 趋势总结
- 关注主题：OpenClaw、AI、AI 绘画、AI 编程
- 强制过滤：色情 / 反动 / 金融 / 加密货币

---

## 📥 安装方法

### Method 1: 批量下载所有技能（推荐）

```bash
# 克隆（含 submodule）
git clone --recursive https://github.com/RAFOLIE/openclaw-skills-Afolie.git

# 复制到 workspace
cp -r openclaw-skills-Afolie/skill-* ~/.openclaw/workspace/skills/
```

### Method 2: 单独安装某个技能

每个技能都有独立仓库，可以单独 clone：

```bash
git clone https://github.com/RAFOLIE/openclaw-skill-twitter-trending.git
cp -r openclaw-skill-twitter-trending ~/.openclaw/workspace/skills/
```

**独立仓库列表**：

| 仓库 | 说明 |
|------|------|
| [openclaw-skill-twitter-trending](https://github.com/RAFOLIE/openclaw-skill-twitter-trending) | 🐦 推文浏览与深度解读 |
| [openclaw-skill-force-tool-first](https://github.com/RAFOLIE/openclaw-skill-force-tool-first) | 🔧 工具优先法则 |
| [openclaw-skill-obsidian-cli](https://github.com/RAFOLIE/openclaw-skill-obsidian-cli) | 💎 Obsidian 笔记操作 |
| [openclaw-skill-creator](https://github.com/RAFOLIE/openclaw-skill-creator) | 🎨 技能创建与优化 |
| [openclaw-skill-vetter](https://github.com/RAFOLIE/openclaw-skill-vetter) | 🔒 技能安全审查 |

> 本仓库通过 Git Submodule 关联各独立仓库，保持双向同步。

## 🔧 依赖工具

| 工具 | 用途 |
|------|------|
| `gh` | GitHub CLI 操作 |
| `obsidian` | Obsidian 笔记操作（CLI） |
| `browser` | OpenClaw 内置浏览器工具 |

## 📝 版本历史

- **2026-03-14** - 更新 twitter-trending v1.6（推荐首页、用户浏览器、过滤增强、拆分 browser-rules.md）
- **2026-03-13** - 新增 twitter-trending v1.1；删除 pinchtab（已卸载）；更新 force-tool-first v1.6
- **2026-03-12** - 新增 skill-creator v1.1（支持从对话历史创建）、skill-vetter v1.0；更新 force-tool-first v1.6
- **2026-03-11** - 添加 pinchtab、skill-creator、skill-vetter
- **2026-03-11** - 创建仓库，上传 force-tool-first v1.5 + obsidian-cli v1.0

## 👤 作者

小企鹅 🐧

## 📄 License

MIT
