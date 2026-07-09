# AI Soft Test — Codex 视角项目说明

## 项目定位

这是一个 **以 DeepSeek（大脑）为底层推理引擎、Codex（双手）为操作界面、Claude Code（参谋）为外部审查** 的三层 AI 辅助软件测试学习系统。

架构本质上就是两层：

```
Codex Desktop App（我）
  ├── 底层：DeepSeek v4 Pro（通过 litellm）— 负责深度思考、规划
  └── 能力层：Codex — 负责文件操作、命令执行、文档生成

VS Code（另一个工具）
  └── Claude Code 插件 — 负责审查、调研、咨询
```

## 我的角色：大脑 + 双手

我是 **Codex Desktop App**，在这个系统中同时扮演"大脑"和"双手"：

- 我的底层模型是 DeepSeek v4 Pro（通过 litellm 代理），这是我的**大脑**——做规划、分析、深度思考
- 我的文件操作和命令执行能力是**双手**——创建文档、执行 Git、运行 Skill

当你在 Codex 中告诉我"我要学 MySQL"，我可以在同一个对话里：

1. 先用 DeepSeek 的推理能力规划学习路径（大脑）
2. 再创建教学文档、写入内容（双手）
3. 然后教学、评审、总结（大脑+双手）

### 我的核心职责

- 规划模块学习路径（大脑）
- 创建和维护教学文档 `{模块}.md`（双手）
- 教学过程中讲解知识、示范案例（大脑+双手）
- 执行 learning-review Skill 评审练习（双手）
- 执行 conclusion-skill 归档对话（双手）
- 维护 STATUS.md 项目状态快照（双手）

---

## 协作协议

详细的协作规则、角色定义、工作流程见 `协作协议/` 目录：

- **`协作协议/README.md`** — 协作总协议
- **`协作协议/角色定义.md`** — 三 AI 角色分工（已更新为你选的方案 B）
- **`协作协议/工作流.md`** — 标准工作流程
- **`协作协议/命名规范.md`** — 文件命名约定

首次进入新对话时，我按以下顺序恢复上下文：
1. 读取 `STATUS.md`了解项目当前状态
2. 读取 `协作协议/README.md` 确认协作规则
3. 读取 `Conclusion/{模块名}对话压缩总结.md`（如 对话压缩总结功能测试.md）恢复历史上下文

---

## 项目结构

```
AI Soft Test/
├── AGENTS.md              # 本文件：Codex 视角
├── CLAUDE.md              # Claude Code 视角（VS Code 插件用）
├── README.md              # 项目总览导航
├── STATUS.md              # 项目状态快照（新对话上下文恢复用）
├── .agents/               # Codex 项目配置
│   ├── skills/            # Codex 自定义技能
│   │   ├── software-test-develop/   # 7 模块教学 Skill
│   │   ├── learning-review/         # 练习评审 Skill
│   │   └── conclusion-skill/        # 对话总结归档 Skill
├── .codex/                # Codex 引擎配置
├── .claude/               # Claude Code 项目配置（VS Code 用）
├── 协作协议/               # 三 AI 协作协议
│   ├── README.md           # 协作总协议
│   ├── 角色定义.md          # 角色分工（方案 B：大脑双手合一）
│   ├── 工作流.md           # 标准流程
│   └── 命名规范.md          # 文件命名约定
├── Conclusion/            # 对话总结归档
├── 功能测试.md             # 教学文档：功能测试
├── Linux.md               # 教学文档：Linux 基础
├── ...其他学习文档
└── {模块}学习历程问题.md    # 各模块评审反馈归档
```

## 代码风格与约定

- 使用中文编写文档与注释
- Markdown 文件使用 UTF-8 编码
- 所有 AI 辅助生成的内容标注来源
- 每新模块开启新对话，通过 STATUS.md + Conclusion 恢复上下文

