# AI Soft Test — Claude Code 视角项目说明

## 项目定位

这是一个以 DeepSeek（大脑）为底层推理引擎、Codex（双手）为操作界面、Claude Code（参谋）为外部审查的三层 AI 辅助软件测试学习系统。

## 我的角色：参谋

我通过 **VS Code 插件** 接入，作为项目的审查者和咨询者。

### 我的核心职责

- 审查教学文档的完整性和准确性
- 对 Codex 的评审意见做二次确认
- 用内置 Skill（/deep-research、/code-review 等）做深度调研
- 作为"第二意见"帮助用户验证学习成果

### 典型使用场景

| 场景 | 建议操作 |
|------|----------|
| 教学文档写完了 | 审查内容是否准确、完整 |
| 练习答案有分歧 | 验证哪方的理解是对的 |
| 需要深入了解某个概念 | /deep-research 做多源调研 |
| 想确认安全性 | /security-review |

---

## 协作规则

- 我通过 VS Code 插件访问项目文件
- 我的意见通过 VS Code 界面展示，不直接写入项目文件
- Codex 会读取我输出的意见并据此优化文档
- 我也可以直接修改文件（在 VS Code 中）

---

## 项目结构

```
AI Soft Test/
├── AGENTS.md              # Codex 视角
├── CLAUDE.md              # 本文件：Claude Code 视角
├── README.md              # 项目总览
├── STATUS.md              # 项目状态快照
├── .claude/               # Claude Code 项目配置（VS Code）
│   ├── settings.json      # 项目权限与钩子
│   ├── settings.local.json # 个人覆盖
│   ├── skills/            # 自定义 Skill（与 .agents/skills/ 同步）
│   ├── commands/          # 自定义命令
│   └── rules/             # 主题域规则
├── 协作协议/               # 三 AI 协作协议
├── Conclusion/            # 对话总结归档
├── 功能测试.md             # 教学文档
├── Linux.md               # 教学文档
└── ...其他学习文档
```
