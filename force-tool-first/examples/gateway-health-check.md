# 示例：检查 OpenClaw Gateway 状态

## 触发关键词
- gateway 状态
- openclaw 服务
- 检查服务是否运行
- gateway 启动了吗

## 问题描述
用户想检查 OpenClaw Gateway 是否正常运行

## 成功的工具调用

### 方法一：检查进程
```json
{
  "tool": "exec",
  "command": "Get-Process | Where-Object {$_.ProcessName -like '*openclaw*'}",
  "platform": "windows"
}
```

### 方法二：检查端口（Gateway 默认 18789）
```json
{
  "tool": "exec",
  "command": "netstat -ano | findstr '18789'",
  "platform": "windows"
}
```

### 方法三：使用 openclaw CLI
```json
{
  "tool": "exec",
  "command": "openclaw gateway status",
  "platform": "any"
}
```

## 成功输出示例

**进程检查**：
```
Handles  NPM(K)    PM(K)      WS(K) VM(M)   CPU(s)     Id ProcessName
-------  ------    -----      ----- -----   ------     -- -----------
    123      15    12345      23456   150    1.23   12345 openclaw
```

**端口检查**：
```
TCP    0.0.0.0:18789          0.0.0.0:0              LISTENING       12345
```

**CLI 检查**：
```
Gateway is running on port 18789
```

## 记录时间
2026-03-11

## 效果
用户确认 Gateway 正在运行，问题解决

## 备注
- 端口 18789 是 Gateway 默认端口
- 如果进程不存在且端口未监听，说明服务未启动
- 启动命令：`openclaw gateway start`
