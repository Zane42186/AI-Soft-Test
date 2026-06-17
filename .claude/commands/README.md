# 项目专属 Commands 目录

将自定义斜杠命令的 `.md` 文件放在此目录下。每个文件即一个命令，文件名（不含扩展名）即为命令名。

## 命令文件格式

```markdown
---
description: 命令简述（显示在自动补全中）
argument-hint: [必选参数] <可选参数>
allowed-tools: Bash(git *), Read, Write
model: claude-sonnet-4-6
---

命令的提示词/指令内容。
使用 $ARGUMENTS 或 $1, $2 引用参数。
```

## 目录组织

支持子目录命名空间：
```
commands/
├── git/
│   ├── commit.md       → /project:git:commit
│   └── pr.md           → /project:git:pr
├── testing/
│   ├── unit.md         → /project:testing:unit
│   └── e2e.md          → /project:testing:e2e
└── review.md           → /review
```

> 注：`commands/` 目录是传统格式，推荐新工作流使用 `skills/` 目录。详见 `skillLearn.md`。
