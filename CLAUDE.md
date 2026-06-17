# AI Soft Test — 项目概述

## 项目定位
这是一个 AI 辅助软件测试的学习与实验项目，用于探索 Claude Code 在软件测试领域的应用。

## 技术栈
- 待定（根据实验方向选择）

## 项目结构
```
AI Soft Test/
├── CLAUDE.md              # 本文件：项目说明与约定
├── .claude/               # Claude Code 项目级配置（沙盒隔离）
│   ├── settings.json      # 项目权限、钩子、环境变量
│   ├── settings.local.json # 个人覆盖配置（不提交 Git）
│   ├── skills/            # 项目专属自定义技能
│   ├── commands/          # 项目专属斜杠命令
│   └── rules/             # 主题域规则文件
├── .mcp.json              # MCP 服务器配置（项目级）
└── skillLearn.md          # Skill 学习指南
```

## 构建与运行
<!-- TODO: 根据项目实际技术栈补充 -->

## 代码风格与约定
- 使用中文编写文档与注释
- 遵循所选用技术栈的社区最佳实践
- 所有 AI 辅助生成的代码需标注来源

## 测试策略
<!-- TODO: 根据项目需求补充 -->

## 注意事项
- 本项目使用项目级 .claude/ 配置实现沙盒隔离
- 所有 Claude Code 行为由项目配置控制，不依赖全局 ~/.claude/
