---
name: pinchtab
description: |
  PinchTab 浏览器自动化工具 - 高性能浏览器控制，支持 AI Agent 操作 Chrome。
  
  **当以下情况使用**：
  - 需要浏览器自动化操作（导航、点击、截图、提取文本）
  - 需要多实例浏览器管理（隔离 profile）
  - 需要 Token 高效的页面操作（比截图便宜 5-13x）
  - 用户提到"浏览器自动化"、"控制 Chrome"、"网页操作"

version: 1.0
metadata:
  openclaw:
    emoji: "🌐"
    priority: high
---

# PinchTab 浏览器自动化技能

## 核心信息

| 项目 | 值 |
|------|-----|
| **版本** | 0.7.8 |
| **端口** | 9867 |
| **大小** | 12MB |
| **Stars** | 6,474 |

## 安装状态

✅ 已安装：`npm install -g pinchtab`

## 快速使用

### 启动服务

```bash
pinchtab
```

服务地址：`http://localhost:9867`

### CLI 命令

| 命令 | 说明 |
|------|------|
| `pinchtab nav <url>` | 导航到 URL |
| `pinchtab snap -i -c` | 获取页面结构（交互元素） |
| `pinchtab click <ref>` | 点击元素（如 e5） |
| `pinchtab text` | 提取页面文本 |
| `pinchtab --help` | 查看帮助 |

### HTTP API

```bash
# 创建实例
curl -X POST http://localhost:9867/instances/launch \
  -H "Content-Type: application/json" \
  -d '{"name":"work","mode":"headless"}'

# 打开标签页
curl -X POST http://localhost:9867/instances/<INST>/tabs/open \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com"}'

# 获取快照
curl "http://localhost:9867/tabs/<TAB>/snapshot?filter=interactive"

# 点击元素
curl -X POST "http://localhost:9867/tabs/<TAB>/action" \
  -H "Content-Type: application/json" \
  -d '{"kind":"click","ref":"e5"}'
```

## 核心概念

| 概念 | 说明 |
|------|------|
| **Server** | 主进程，管理 profiles、instances、dashboard |
| **Instance** | 运行中的 Chrome 进程 |
| **Profile** | 浏览器状态（cookies、历史、存储） |
| **Tab** | 单个网页 |
| **Bridge** | 单实例运行时（通常由 server 管理） |

## 使用场景

1. **浏览器自动化** - 导航、点击、表单填写
2. **数据提取** - 高效提取页面文本（比截图便宜）
3. **多账号管理** - 隔离的 profile 管理
4. **无头模式** - 服务器端自动化
5. **AI Agent** - 让 AI 控制浏览器

## 与 OpenClaw 浏览器工具的区别

| 功能 | OpenClaw browser | PinchTab |
|------|-----------------|----------|
| **用途** | 通用浏览器控制 | 高性能自动化 |
| **Token 消耗** | 截图为主 | 文本提取（更省） |
| **多实例** | 有限支持 | 原生支持 |
| **Profile 隔离** | 有限 | 完整支持 |
| **无头模式** | 支持 | 原生优化 |

## 文档

- 官网：https://pinchtab.com
- 文档：https://pinchtab.com/docs
- GitHub：https://github.com/pinchtab/pinchtab
