# OpenClaw Skills - Afolie

OpenClaw 技能备份仓库

## 技能列表

### 1. force-tool-first 🔧

**版本**: 1.5

强制优先调用本地工具检查环境的技能（支持自学习优化）。

**核心功能**:
- 工具优先法则（永不猜测，永远执行工具）
- 自学习迭代（成功案例自动记录）
- Windows 平台适配（PowerShell 命令）
- 软件安装支持（winget、环境变量刷新）
- pty 交互式命令处理
- **必须使用专用 CLI**（Obsidian → obsidian-cli，GitHub → gh）

**触发场景**:
- Git & GitHub 操作
- 软件安装
- 环境检查
- 系统状态
- 文件操作
- Obsidian 笔记

**示例库**:
- `git-status-check.md` - Git 仓库状态
- `github-cli-list-repos.md` - GitHub 远程仓库
- `software-install-winget.md` - 软件安装
- `gateway-health-check.md` - 服务状态检查
- `file-list.md` - 文件列表查看

### 2. obsidian-cli 💎

**版本**: 1.0

Obsidian 笔记操作技能 - 必须通过 obsidian-cli 完成。

**核心原则**: 所有 Obsidian 笔记操作必须使用 obsidian-cli，禁止直接修改文件。

**功能**:
- 搜索笔记（按名称或内容）
- 创建笔记
- 移动/重命名笔记（自动更新链接）
- 删除笔记

**为什么必须用 CLI**:
- 自动更新 `[[wikilinks]]` 链接
- 保持 Obsidian 元数据完整
- 触发插件和同步
- 维护笔记关系图

## 安装方法

将技能文件夹复制到 OpenClaw skills 目录：

```bash
# Windows
cp -r force-tool-first "$env:APPDATA\npm\node_modules\openclaw\skills\"
cp -r obsidian-cli "$env:APPDATA\npm\node_modules\openclaw\skills\"

# macOS/Linux
cp -r force-tool-first ~/.openclaw/skills/
cp -r obsidian-cli ~/.openclaw/skills/
```

## 依赖工具

- `gh` (GitHub CLI) - 用于 GitHub 操作
- `obsidian-cli` - 用于 Obsidian 笔记操作
- `winget` (Windows) - 用于软件安装

## 版本历史

- **v1.5** (2026-03-11) - 添加必须使用专用 CLI 的铁律
- **v1.4** (2026-03-11) - 添加 Obsidian CLI 支持
- **v1.3** (2026-03-11) - 添加 GitHub 克隆确认流程
- **v1.0** (2026-03-11) - 初始版本

## 作者

小企鹅 🐧

## License

MIT
