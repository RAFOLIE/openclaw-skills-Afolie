# 示例：检查 Git 仓库状态

## 触发关键词
- git 状态
- 仓库状态
- 有什么改动
- 需要提交吗
- 当前分支

## 问题描述
用户想查看当前 Git 仓库的状态、分支、未提交的改动等

## 成功的工具调用

### 方法一：查看完整状态
```json
{
  "tool": "exec",
  "command": "git status",
  "platform": "any"
}
```

### 方法二：查看当前分支
```json
{
  "tool": "exec",
  "command": "git branch --show-current",
  "platform": "any"
}
```

### 方法三：查看简要状态
```json
{
  "tool": "exec",
  "command": "git status -sb",
  "platform": "any"
}
```

### 方法四：查看远程同步状态
```json
{
  "tool": "exec",
  "command": "git fetch --dry-run 2>&1; git status",
  "platform": "any"
}
```

## 成功输出示例

**完整状态**：
```
On branch main
Your branch is up to date with 'origin/main'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        new-file.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

**简要状态**：
```
## main...origin/main
 M README.md
?? new-file.txt
```

## 记录时间
2026-03-11

## 效果
用户了解当前仓库状态，知道有哪些文件需要提交

## 备注
- `M` = modified（已修改）
- `A` = added（已添加）
- `D` = deleted（已删除）
- `??` = untracked（未跟踪）
