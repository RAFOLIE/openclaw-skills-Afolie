---
name: pinchtab
description: |
  PinchTab 浏览器自动化工具 - 高性能浏览器控制，支持 AI Agent 操作 Chrome。
  
  **当以下情况使用**：
  - 需要浏览器自动化操作（导航、点击、截图、提取文本）
  - 需要多实例浏览器管理（隔离 profile）
  - 需要 Token 高效的页面操作（比截图便宜 5-13x）
  - 用户提到"浏览器自动化"、"控制 Chrome"、"网页操作"、"PinchTab"

version: 1.2
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
| **安装** | `npm install -g pinchtab` |

## Windows 环境配置

### 配置文件位置
`C:\Users\<用户名>\.pinchtab\config.json`

### Chrome 浏览器配置
```json
{
  "browser": {
    "executablePath": "C:\\Program Files\\Google\\Chrome\\Application\\chrome.exe"
  },
  "server": {
    "bind": "127.0.0.1",
    "port": 9867
  }
}
```

### 启动服务（Windows）
```powershell
# 方法 1: 直接启动（推荐）
node "$env:APPDATA\npm\node_modules\pinchtab\bin\pinchtab" --port 9867

# 方法 2: 后台启动
Start-Process -FilePath "node" -ArgumentList "$env:APPDATA\npm\node_modules\pinchtab\bin\pinchtab --port 9867" -WindowStyle Hidden
```

## 快速使用

### 1. 启动服务并创建实例

```powershell
# 启动 PinchTab（在一个终端）
pinchtab

# 创建浏览器实例（PowerShell）
$body = @{name='work'; mode='headed'} | ConvertTo-Json
Invoke-RestMethod -Uri 'http://localhost:9867/instances/launch' -Method POST -Body $body -ContentType 'application/json'
```

### 2. 打开网页并获取快照

```powershell
# 打开网页
$body = @{url='https://example.com'} | ConvertTo-Json
$result = Invoke-RestMethod -Uri 'http://localhost:9867/instances/<INST>/tabs/open' -Method POST -Body $body -ContentType 'application/json'
$tabId = $result.tabId

# 获取页面快照（交互元素）
$snapshot = Invoke-RestMethod -Uri "http://localhost:9867/tabs/$tabId/snapshot?filter=interactive" -Method GET
$snapshot.nodes | Format-Table ref, role, name
```

### 3. 点击和操作

```powershell
# 点击元素（例如 e5）
$body = @{kind='click'; ref='e5'} | ConvertTo-Json
Invoke-RestMethod -Uri "http://localhost:9867/tabs/$tabId/action" -Method POST -Body $body -ContentType 'application/json'

# 输入文本
$body = @{kind='type'; ref='e3'; text='search query'} | ConvertTo-Json
Invoke-RestMethod -Uri "http://localhost:9867/tabs/$tabId/action" -Method POST -Body $body -ContentType 'application/json'

# 提取页面文本（Token 高效）
$text = Invoke-RestMethod -Uri "http://localhost:9867/tabs/$tabId/text" -Method GET
```

## 常用 API 端点

### 实例管理

| 端点 | 方法 | 说明 |
|------|------|------|
| `/instances/launch` | POST | 创建实例（`{name, mode: "headed"\|"headless"}`） |
| `/instances` | GET | 列出所有实例 |
| `/instances/<INST>` | DELETE | 停止并删除实例 |

### 标签页管理

| 端点 | 方法 | 说明 |
|------|------|------|
| `/instances/<INST>/tabs/open` | POST | 打开标签（`{url}`） |
| `/tabs/<TAB>/snapshot` | GET | 获取页面快照 |
| `/tabs/<TAB>/action` | POST | 执行操作（click、type、press 等） |
| `/tabs/<TAB>/text` | GET | 提取页面文本 |

## 核心概念

| 概念 | 说明 |
|------|------|
| **Server** | 主进程，管理 profiles、instances、dashboard |
| **Instance** | 运行中的 Chrome 进程 |
| **Profile** | 浏览器状态（cookies、历史、存储） |
| **Tab** | 单个网页 |
| **Ref** | 元素引用（如 e5、e12），用于点击和操作 |

## 实际使用经验

### ✅ 最佳实践

1. **服务稳定性（重要！）**
   - ⚠️ PinchTab 服务可能意外退出（进程 code=1）
   - ✅ **推荐：使用长时间 timeout**（如 600-900 秒）
   - ✅ **推荐：每次操作前检查服务状态**
   - ✅ **推荐：实现自动重启机制**
   ```powershell
   # 检查服务是否运行
   if (-not (Test-NetConnection localhost 9867 -InformationLevel Quiet)) {
       # 重启服务
       node "$env:APPDATA\npm\node_modules\pinchtab\bin\pinchtab" --port 9867
       Start-Sleep -Seconds 5
   }
   ```

2. **浏览器实例管理**
   - ⚠️ **不要自动关闭浏览器**（用户可能正在登录）
   - ✅ 使用 headed 模式更容易调试
   - ✅ 保持实例运行直到用户明确要求关闭
   - ✅ 使用有意义的 profile 名称

3. **Token 优化**
   - ✅ 使用 `text` 端点提取文本（比截图便宜 5-13x）
   - ✅ 使用 `filter=interactive` 只获取交互元素
   - ✅ 避免不必要的快照

4. **Twitter 使用偏好**
   - ✅ **优先使用"热门"（Top）搜索**：`f=top`
   - ⚠️ 避免"最新"（Latest）搜索，质量较低
   - ✅ 示例：`https://twitter.com/search?q=openclaw&f=top`

### ⚠️ 常见问题

**Q: 服务意外退出（Process exited with code 1）**
- 这是已知问题，需要重启服务
- **解决方案**：实现自动重启机制
- **预防**：使用更长的 timeout（600-900 秒）

**Q: 实例启动失败（status: error）**
- 检查 Chrome 是否正确安装
- 检查配置文件路径是否正确
- 查看 PinchTab 日志

**Q: 无法连接到服务**
- 确认服务已启动：`Test-NetConnection localhost 9867`
- 检查端口是否被占用
- **尝试重启服务**（最常见解决方案）

**Q: PowerShell 中的 JSON 编码**
- 使用 `ConvertTo-Json` 转换对象
- 设置 `-ContentType 'application/json'`

## 与 OpenClaw 浏览器工具的区别

| 功能 | OpenClaw browser | PinchTab |
|------|-----------------|----------|
| **用途** | 通用浏览器控制 | 高性能自动化 |
| **Token 消耗** | 截图为主 | 文本提取（更省） |
| **多实例** | 有限支持 | 原生支持 |
| **Profile 隔离** | 有限 | 完整支持 |
| **无头模式** | 支持 | 原生优化 |
| **Windows 支持** | ✅ 原生 | ⚠️ 需配置 |

## 使用场景

1. **浏览器自动化** - 导航、点击、表单填写
2. **数据提取** - 高效提取页面文本（比截图便宜）
3. **多账号管理** - 隔离的 profile 管理
4. **无头模式** - 服务器端自动化
5. **AI Agent** - 让 AI 控制浏览器

## 文档

- 官网：https://pinchtab.com
- 文档：https://pinchtab.com/docs
- GitHub：https://github.com/pinchtab/pinchtab

---

**版本历史**：
- v1.2 (2026-03-12): 增强服务稳定性管理、添加自动重启机制、Twitter 使用偏好（热门而非最新）、实际使用经验更新
- v1.1 (2026-03-12): 添加 Windows 环境配置、PowerShell 示例、实际使用经验
- v1.0: 初始版本
