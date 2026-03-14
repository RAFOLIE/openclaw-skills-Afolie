# 示例：列出目录文件

## 触发关键词
- 有什么文件
- 列出文件
- 文件夹内容
- 当前目录
- 目录列表

## 问题描述
用户想查看某个目录下的文件和文件夹

## 成功的工具调用

### 方法一：PowerShell 列出当前目录
```json
{
  "tool": "exec",
  "command": "Get-ChildItem | Select-Object Name, Length, LastWriteTime",
  "platform": "windows"
}
```

### 方法二：列出指定目录
```json
{
  "tool": "exec",
  "command": "Get-ChildItem -Path \"C:\\Users\\win11-001\\.openclaw\\workspace\" | Select-Object Name, Length, LastWriteTime",
  "platform": "windows"
}
```

### 方法三：递归列出（包含子目录）
```json
{
  "tool": "exec",
  "command": "Get-ChildItem -Recurse -Depth 1 | Select-Object FullName",
  "platform": "windows"
}
```

### 方法四：只列出文件夹
```json
{
  "tool": "exec",
  "command": "Get-ChildItem -Directory | Select-Object Name",
  "platform": "windows"
}
```

## 成功输出示例

```
Name              Length    LastWriteTime
----              ------    -------------
AGENTS.md           3531    2026/3/11 15:00
IDENTITY.md          256    2026/3/9 23:58
MEMORY.md           1024    2026/3/11 14:30
USER.md              128    2026/3/9 23:58
memory           <DIR>     2026/3/11 12:00
```

## 记录时间
2026-03-11

## 效果
用户看到目录内容，了解有哪些文件可用

## 备注
- `<DIR>` 表示文件夹
- `Length` 是文件大小（字节）
- 可以用 `-Name` 参数只显示名称
