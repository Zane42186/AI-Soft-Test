# Claude Code Skills 学习指南

## 什么是 Skill？

Skill（技能）是 Claude Code 中的专用功能模块，每个 Skill 针对特定任务场景进行了优化。它们类似于"领域专家"，当你的任务匹配某个 Skill 的专长时，Claude 会自动调用对应 Skill 来处理任务，提供更专业、更高质量的结果。

> 💡 **简单理解**：Skill = 预配置的专业工作流程。你只需用 `/技能名` 或自然语言描述任务，Claude 便会自动匹配合适的 Skill 来执行。

---

## 可用的 Skills 一览

| Skill 名称 | 触发方式 | 用途简介 |
|------------|----------|----------|
| `deep-research` | `/deep-research` | 深度调研：多源搜索、交叉验证、生成引用报告 |
| `code-review` | `/code-review` | 代码审查：检查 diff 中的 bug、简化机会、效率问题 |
| `simplify` | `/simplify` | 代码简化：对已修改代码进行复用、简化、效率优化 |
| `review` | `/review` | PR 审查：审查 Pull Request |
| `security-review` | `/security-review` | 安全审查：对当前分支的变更进行安全检查 |
| `verify` | `/verify` | 验证变更：运行应用并观察行为，确认代码修改有效 |
| `run` | `/run` | 启动应用：启动并驱动项目应用以查看变更效果 |
| `init` | `/init` | 初始化项目文档：为新项目创建 CLAUDE.md 代码库文档 |
| `loop` | `/loop` | 循环任务：按固定间隔重复执行某个命令或提示 |
| `update-config` | `/update-config` | 配置管理：修改 settings.json 中的设置、钩子、权限等 |
| `keybindings-help` | `/keybindings-help` | 按键绑定：自定义键盘快捷键和组合键 |
| `claude-api` | `/claude-api` | API 参考：查阅 Claude API / Anthropic SDK 的文档 |
| `fewer-permission-prompts` | `/fewer-permission-prompts` | 权限优化：扫描历史记录，减少不必要的权限提示 |

---

## 如何使用 Skill

### 方式一：斜杠命令（推荐新手）

在对话输入框中直接输入以 `/` 开头的命令即可触发 Skill：

```
/deep-research 量子计算的最新进展
/code-review
/init
/loop 5m /status
```

### 方式二：自然语言描述

你不需要记住所有命令名。用自然语言描述你想做什么，Claude 会自动判断并调用合适的 Skill：

| 你说的话 | Claude 自动调用的 Skill |
|----------|------------------------|
| "帮我深入调研一下微服务架构的最佳实践" | `deep-research` |
| "审查一下我刚才的代码改动" | `code-review` |
| "简化这段代码但不改变功能" | `simplify` |
| "安全检查当前分支" | `security-review` |
| "验证这个修复是否真的有效" | `verify` |
| "帮我运行这个应用看看效果" | `run` |
| "为新项目初始化文档" | `init` |
| "每 5 分钟检查一次部署状态" | `loop` |
| "允许 npm 命令不用每次都确认" | `update-config` |
| "修改快捷键 Ctrl+S 为提交" | `keybindings-help` |
| "Claude API 的最新定价是多少" | `claude-api` |

---

## 各 Skill 详细说明

### 1. `deep-research` — 深度调研

**适用场景**：需要多源查证、深度分析、输出引用报告的研究任务。

```
/deep-research 对比 React 19 和 Vue 3.5 在性能、生态和开发体验上的差异
```

**工作流程**：
1. 多路搜索引擎同时搜索不同关键词
2. 抓取相关网页内容
3. 对抗性验证（交叉核实信息来源）
4. 综合生成带引用的调研报告

**提示**：问题越具体，调研质量越高。如果问题模糊（如"买什么车好"），Claude 会先追问你预算、用途、地区等信息。

---

### 2. `code-review` — 代码审查

**适用场景**：审查暂存区或工作区的代码变更。

```
/code-review            # 默认审查级别
/code-review --comment  # 以行内评论形式输出审查意见
/code-review --fix      # 直接应用修复建议
```

**审查维度**：
- 🐛 **Bug 检测**：逻辑错误、边界条件、空值处理
- ♻️ **复用性**：重复代码、可抽取的公共逻辑
- 📐 **简化机会**：过于复杂的实现、可读性改进
- ⚡ **效率**：性能瓶颈、不必要的计算

---

### 3. `simplify` — 代码简化

**适用场景**：对已修改的代码进行质量优化（不找 bug，专注代码质量）。

```
/simplify
```

**与 `code-review` 的区别**：
- `code-review`：找 bug + 找优化机会（全面审查）
- `simplify`：只做优化，不找 bug（专注代码质量）

---

### 4. `review` — PR 审查

**适用场景**：审查 GitHub Pull Request。

```
/review
```

---

### 5. `security-review` — 安全审查

**适用场景**：对当前分支的变更进行全面的安全审计。

```
/security-review
```

**检查内容**：注入漏洞、认证绕过、敏感信息泄露、不安全的依赖等。

---

### 6. `verify` — 验证变更

**适用场景**：运行应用并实际观察，确保代码修改达到预期效果。

```
/verify
```

**典型用法**：改完代码后，不确定是否真的修好了 bug → 用 `/verify` 让 Claude 运行应用并确认。

---

### 7. `run` — 启动应用

**适用场景**：需要启动项目应用来查看变更效果。

```
/run
```

**工作方式**：先查找项目中是否有专门启动应用的 Skill，如果没有则根据项目类型（CLI、Web 服务、TUI、Electron 等）自动选择启动方式。

---

### 8. `init` — 初始化 CLAUDE.md

**适用场景**：为新项目创建 `CLAUDE.md` 文件，记录代码库的关键信息。

```
/init
```

**生成内容**：项目架构、构建命令、代码风格、关键约定等。这份文件会帮助 Claude 更好地理解你的项目。

---

### 9. `loop` — 循环任务

**适用场景**：需要定期重复执行某个操作。

```
/loop 5m /status          # 每 5 分钟检查一次状态
/loop 30m /test           # 每 30 分钟运行一次测试
/loop                     # 默认每 10 分钟重复上一次任务
```

**时间格式**：`s`（秒）、`m`（分）、`h`（时），如 `30s`、`5m`、`2h`。

---

### 10. `update-config` — 配置管理

**适用场景**：修改 Claude Code 的配置文件（settings.json）。

```
/update-config allow npm commands       # 允许 npm 命令自动执行
/update-config add permission for git   # 添加 git 权限
/update-config set DEBUG=true           # 设置环境变量
```

**注意**：对于自动行为配置（"以后每次 X 时做 Y"）、权限设置、环境变量、钩子故障排查，必须使用此 Skill——Claude 的记忆功能无法替代配置文件。

---

### 11. `keybindings-help` — 按键绑定

**适用场景**：自定义键盘快捷键。

```
/keybindings-help                        # 查看帮助
/keybindings-help rebind ctrl+s          # 重新绑定 Ctrl+S
/keybindings-help add chord shortcut     # 添加组合快捷键
```

---

### 12. `claude-api` — Claude API 参考

**适用场景**：查询 Claude API 或 Anthropic SDK 的相关信息。

触发此 Skill 的关键词：Claude、Anthropic、Opus、Sonnet、Haiku、`anthropic`、`@anthropic-ai`、token 计数、模型迁移等。

```
/claude-api 最新模型有哪些
/claude-api streaming 怎么用
/claude-api tool use 的参数格式
```

---

### 13. `fewer-permission-prompts` — 减少权限提示

**适用场景**：分析历史对话记录，找出频繁出现的只读命令，自动添加到权限白名单中。

```
/fewer-permission-prompts
```

**效果**：减少日常操作中不必要的"是否允许执行此命令"的确认弹窗。

---

## 进阶技巧

### 1. Skill 的自动匹配

你不需要精确记住每个 Skill 的名字。Claude 会根据你的意图自动判断。例如：

- "帮我研究一下……" → 触发 `deep-research`
- "看看这段代码有没有问题" → 触发 `code-review`
- "这个改动安全吗" → 触发 `security-review`

### 2. 组合使用 Skills

多个 Skill 可以串联使用，形成高效工作流：

```
1. /init                          # 先初始化项目文档
2. 开发代码...
3. /code-review                   # 审查代码改动
4. /verify                        # 验证修改效果
5. /security-review               # 安全检查
```

### 3. 循环任务的高级用法

```bash
# 每 5 分钟检查 CI 状态
/loop 5m gh pr checks

# 每 30 分钟拉取最新代码并运行测试
/loop 30m git pull && npm test
```

---

## 自定义 Skill：存放与管理

除了使用内置 Skill，你还可以创建自己的 Skill。了解 Skill 的存放位置和目录选择是高效管理 Skill 的关键。

### Skill 的存放位置

Claude Code 支持三个级别的 Skill 存放目录：

| 级别 | 存放路径 | 作用范围 | 典型用途 |
|------|----------|----------|----------|
| **项目级** | `<项目根>/.claude/skills/<名称>/SKILL.md` | 仅当前项目 | 团队共享的项目专属工作流 |
| **用户级（全局）** | `~/.claude/skills/<名称>/SKILL.md` | 所有项目 | 个人通用的开发辅助工具 |
| **插件级** | 由插件提供 | 安装插件的项目 | 第三方扩展功能 |

#### 操作系统对应的用户级路径

| 操作系统 | 用户级 Skills 路径 |
|----------|-------------------|
| **Windows（原生）** | `C:\Users\<用户名>\.claude\skills\` |
| **Windows（WSL）** | `~/.claude/skills/`（WSL 内部） |
| **macOS** | `/Users/<用户名>/.claude/skills/` |
| **Linux** | `/home/<用户名>/.claude/skills/` |

### 优先级顺序（同名冲突时）

当多个位置存在同名 Skill 时，按以下优先级使用：

```
🥇 Enterprise（企业托管）    ← 最高优先级
🥈 用户级（~/.claude/skills/）
🥉 项目级（.claude/skills/）
🏅 插件级（命名空间：plugin-name:skill-name）  ← 最低优先级
```

> 💡 **实际意义**：你可以用个人 Skill 覆盖项目 Skill，用企业 Skill 覆盖个人 Skill。插件 Skill 始终以 `插件名:技能名` 的形式调用，不会与其他级别冲突。

---

### Skill 的目录结构

一个标准的自定义 Skill 目录结构如下：

```
.claude/skills/<skill-名称>/
├── SKILL.md              # ✅ 必需：核心文件，包含 YAML 头信息和指令
├── references/           # 可选：深度参考资料
│   ├── api-docs.md
│   └── conventions.md
├── scripts/              # 可选：辅助脚本
│   └── helper.py
└── examples/             # 可选：示例输出
    └── sample-output.md
```

**最小结构**（只有一个文件即可生效）：

```
.claude/skills/my-skill/
└── SKILL.md
```

---

### SKILL.md 文件格式

`SKILL.md` 是 Skill 的核心文件，由两部分组成：

#### （1）YAML 前置信息（Frontmatter）

```yaml
---
name: my-skill              # 必需：Skill 名称（用于 /名称 调用）
description: 一句话描述       # 必需：显示在自动补全和帮助中
allowed-tools: Read, Write, Bash(git *)  # 可选：限制可用的工具
model: claude-sonnet-4-5     # 可选：指定模型（建议省略以继承会话模型）
---
```

#### （2）Markdown 指令正文

YAML 块之后是给 Claude 的实际指令，使用标准 Markdown 编写。

**完整示例** — 一个用于生成 Git 提交信息的 Skill：

```markdown
---
name: git-commit
description: 分析暂存区变更并生成规范的 Git 提交信息
allowed-tools: Bash(git *), Read
---

# Git Commit 信息生成器

## 任务
分析当前 `git diff --staged` 的输出，生成一条符合 Conventional Commits 规范的提交信息。

## 规则
1. 类型必须从以下选择：feat, fix, docs, style, refactor, perf, test, chore, ci
2. 标题不超过 72 个字符
3. 正文用中文描述变更内容和原因
4. 如果有破坏性变更，加上 BREAKING CHANGE 脚注

## 示例输出
` ` ` 
feat: 添加用户登录功能

实现了基于 JWT 的用户认证系统，包括登录接口和中间件。
` ` `
```

---

### Commands 与 Skills 的区别

Claude Code 有两种自定义工作流的方式，了解区别有助于选择合适的格式：

| 特性 | Commands（命令） | Skills（技能） |
|------|-----------------|---------------|
| **存放位置** | `.claude/commands/*.md` | `.claude/skills/<名称>/SKILL.md` |
| **存储方式** | 单文件 `.md` | 目录形式（可含多个附属文件） |
| **触发方式** | 仅手动 `/命令` 调用 | 手动 `/技能名` + 自动上下文匹配 |
| **支持参数** | ✅ `$ARGUMENTS`, `$1`, `$2` | ❌ 无内置参数传递 |
| **支持附属文件** | ❌ 单文件限制 | ✅ 可附带脚本、参考文档、示例 |
| **适合场景** | 简单、一次性、带参数的任务 | 复杂、多步骤、需参考资料的任务 |
| **状态** | 传统格式 | ✅ 推荐的现代格式 |

#### 如何选择？

- **用 Command**：快速生成一条 SQL、格式化一段 JSON、带参数的简单操作
- **用 Skill**：完整的代码审查流程、项目脚手架生成、需要规则手册的复杂任务

> 📌 **注意**：`.claude/commands/` 目录被视为传统格式，官方推荐新项目使用 `.claude/skills/` 目录格式。两者加载机制相同，只有文件布局不同。

---

### 存放目录选择指南

#### 选择「项目级」（`.claude/skills/`）当：

- ✅ 该工作流与项目架构/约定强相关（如"为此项目添加新 API 端点"）
- ✅ 需要团队所有成员使用相同的标准化流程
- ✅ 希望通过 Git 与团队共享工作流
- ✅ Skill 引用了项目内的特定文件或路径

```bash
# 创建项目级 Skill
mkdir -p .claude/skills/api-generator
touch .claude/skills/api-generator/SKILL.md
```

#### 选择「用户级」（`~/.claude/skills/`）当：

- ✅ 该工作流在任何项目中都能通用（如"生成 Git 提交信息"）
- ✅ 属于个人的开发习惯和偏好
- ✅ 不想把它提交到某个项目的 Git 仓库中
- ✅ 涉及个人密钥或敏感配置

```bash
# 创建用户级 Skill（Windows 原生）
mkdir -p C:\Users\lenovo\.claude\skills\my-helper
# 或使用 %USERPROFILE%
mkdir -p %USERPROFILE%\.claude\skills\my-helper
```

> ⚠️ **注意**：Skill 添加后需要**重启 Claude Code** 才能被检测和加载。

---

### 快速上手：创建你的第一个自定义 Skill

```bash
# 1. 创建目录
mkdir -p .claude/skills/hello-world

# 2. 创建 SKILL.md
cat > .claude/skills/hello-world/SKILL.md << 'EOF'
---
name: hello-world
description: 一个简单的示例 Skill，向用户问好
---

# Hello World Skill

当用户调用此 Skill 时，请用友好的语气向用户问好，
并询问今天想做什么开发任务。

你可以根据当前项目的情况给出针对性的建议。
EOF

# 3. 重启 Claude Code

# 4. 在对话中输入 /hello-world 测试效果
```

### 额外提示：Skill 编写最佳实践

1. **一个 Skill 只做一件事** — 保持专注，职责单一
2. **名称用 kebab-case** — 如 `code-review`、`fix-lint`，不使用空格或下划线
3. **精确限制工具权限** — 用 `Bash(git *)` 而非 `Bash(*)`，提高安全性
4. **指令保持简洁** — 100-500 词最佳，详细参考资料放入 `references/` 目录
5. **提供示例输出** — 在 `examples/` 目录中放入期望的输出格式，帮助 Claude 生成一致的结果
6. **条件允许时继承模型** — 省略 `model` 字段，让 Skill 使用当前会话的模型设置

---

## 总结

| 我想做什么 | 用这个 Skill |
|------------|-------------|
| 深度研究某个话题 | `deep-research` |
| 审查代码改动 | `code-review` |
| 简化/优化代码 | `simplify` |
| 审查 PR | `review` |
| 安全检查 | `security-review` |
| 确认修改是否生效 | `verify` |
| 运行应用看效果 | `run` |
| 初始化项目文档 | `init` |
| 定时重复执行任务 | `loop` |
| 修改 Claude Code 配置 | `update-config` |
| 自定义快捷键 | `keybindings-help` |
| 查 API 文档 | `claude-api` |
| 减少权限弹窗 | `fewer-permission-prompts` |

> 💡 **最佳实践**：先用自然语言描述任务，让 Claude 帮你选择合适的 Skill。等你熟悉了，再用 `/命令` 提高效率。
