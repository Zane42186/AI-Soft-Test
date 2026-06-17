# 项目专属 Skills 目录

将自定义 Skill 放在此目录下。每个 Skill 是一个子目录，其中 `SKILL.md` 是唯一必需的文件。

## 目录结构

```
skills/<skill-名称>/
├── SKILL.md              # ✅ 必需：YAML 头信息 + Markdown 指令
├── references/           # 可选：深度参考资料
├── scripts/              # 可选：辅助脚本
└── examples/             # 可选：示例输出
```

## SKILL.md 最小模板

```markdown
---
name: my-skill
description: 一句话描述 Skill 的用途
allowed-tools: Read, Write, Bash(git *)
---

# Skill 名称

当用户调用此 Skill 时，请按照以下步骤操作：
1. 步骤一
2. 步骤二
...
```

## 优先级

当同名 Skill 在多个位置存在时：
1. Enterprise（企业托管）— 最高
2. 用户级（~/.claude/skills/）
3. 项目级（.claude/skills/）← 当前目录
4. 插件级 — 最低

详见项目根目录下的 `skillLearn.md`。
