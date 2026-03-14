# 浏览器使用规则

## 两种模式对比

| 模式 | profile | 登录态 | 可见性 | 适用场景 |
|------|---------|--------|--------|---------|
| **用户浏览器** | `user` | ✅ 有 | 在用户窗口操作 | 需要登录态（Twitter、GitHub 等） |
| **隔离浏览器** | `openclaw` | ❌ 无 | 独立窗口 | 不需要登录的网页抓取 |

## 默认策略

**优先使用 `profile="user"`**，因为用户的 Chrome 常开且已登录常用网站。

`profile="openclaw"` 作为备选方案：当 `user` 连不上、不需要登录态、或用户明确要求安静操作时使用。

## 操作规则（必须遵守）

### 1. 不新建标签页

- **禁止**：`browser action=open profile=user`
- **改用**：`browser action=navigate` 在已有标签页内导航
- 原因：突然新建标签页会打断用户的浏览

### 2. 找到目标标签页

先列出现有标签页：
```
browser action=tabs profile=user
```

根据 URL 找到对应的 `targetId`，后续所有操作都用这个 ID。

### 3. X (Twitter) 标签页

- 用户常开 X，直接在 X 标签页内操作
- 搜索推文：`navigate targetUrl=https://x.com/search?q=...` 到 X 标签页
- 回到首页：`navigate targetUrl=https://x.com/home` 到 X 标签页

### 4. 操作顺序

1. `tabs` → 找到目标标签页 ID
2. `navigate` → 在该标签页内导航
3. `act/wait` → 等待加载
4. `snapshot` / `screenshot` → 获取内容
5. 如需交互：`act/click` / `act/type` 等

## Chrome 前提条件

Chrome 需要以远程调试模式启动：

```
chrome.exe --remote-debugging-port=9222
```

- Windows 路径：`C:\Program Files\Google\Chrome\Application\chrome.exe`
- 用户数据目录：`%LOCALAPPDATA%\Google\Chrome\User Data`
- 用户已启用（截至 2026-03-14）

### 连接失败排查

| 错误信息 | 原因 | 解决方案 |
|---------|------|---------|
| `DevToolsActivePort not found` | Chrome 未带调试参数启动 | 关闭 Chrome，用 `--remote-debugging-port=9222` 重启 |
| `Could not connect to Chrome` | Chrome 未运行 | 启动 Chrome |
| 连接超时 | 端口被占用或防火墙 | 检查 9222 端口是否可用 |

## 注意事项

- 操作用户浏览器时**只看不操作**（除非用户明确要求）
- 不要关闭用户打开的标签页
- 不要在用户的 OpenClaw 管理页签里操作
- 截图和快照可以在后台进行，不会切换窗口
