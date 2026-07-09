---
name: conclusion-skill
description: 对话总结与归档技能 — 将当前对话内容自动总结为结构化 Markdown 文档，支持增量/全量两种模式，并自动提交推送至远程 GitHub 仓库。触发词：「总结当前内容」。
allowed-tools: Read, Write, Bash(git *), Bash(ssh *), Bash(ls *), Bash(cat *), Bash(mkdir *), Bash(find *)
---

# ConclusionSkill — 对话总结与归档

## 运行环境
- Codex + 后端模型
- 本地具备 Git、SSH 环境
- 远程仓库：`git@github.com:Zane42186/AI-Soft-Test.git`

---

## 一、核心触发规则

### 1.1 主动触发条件
- 用户输入明文指令：「**总结当前内容**」，自动执行总结逻辑；
- 也支持用户以任意等效表述触发，如"帮我做个总结""归档当前对话"等。

### 1.2 去重机制
- 若用户在 30 秒内连续重复发送「总结当前内容」或等效指令，仅执行一次，忽略后续重复调用。
- 执行过程中不再接收新的总结指令，直至当前流程结束。

### 1.3 内容截取规则（⚠️ 核心，必须严格遵守）

**读取状态文件：**
首先读取 `Conclusion/.summary_record.json`，获取以下字段：
- `lastConversationId` — 上次总结的对话 ID
- `lastMessageIndex` — 上一轮总结截止的消息轮次索引
- `maxFileNumber` — 当前最大文件序号
- `currentConversationId` — 当前对话 ID（由 Codex 会话 UUID 标识）

**截取逻辑：**

| 场景 | 判断条件 | 行为 |
|------|----------|------|
| **全量总结** | `.summary_record.json` 不存在，或 `currentConversationId ≠ lastConversationId`（新对话） | 总结本次完整对话的全部内容（所有用户提问 + 所有 AI 回复） |
| **增量总结** | `.summary_record.json` 存在，且 `currentConversationId = lastConversationId`（同一对话） | 仅截取 [lastMessageIndex+1] 到当前最新轮次的全部新增对话内容 |

---

## 二、本地文件存储规范

### 2.1 存储根目录
- 固定路径：项目根目录下的 `Conclusion/` 文件夹
- 运行时自动检查目录是否存在，不存在则 `mkdir -p Conclusion`

### 2.2 文件命名规则
- 格式：`00x总结.md`，x 为十进制自增数字，固定 3 位数字补零
- 序号生成流程：
  1. 扫描 `Conclusion/` 目录内所有匹配 `^[0-9]{3}总结\.md$` 命名规则的文件
  2. 提取文件名中的数字部分，取最大值 +1
  3. 无文件则初始值 x=1，生成 `001总结.md`
  4. 同时从 `.summary_record.json` 的 `maxFileNumber` 字段交叉验证，取两者较大值 +1

### 2.3 Markdown 文档强制模板

生成的每个 `00x总结.md` 文件必须严格遵循以下结构并写入：

```markdown
# 00x对话总结

## 1. 总结区间说明
- **是否增量总结**：是 / 否
- **截取范围**：xxx（"全新对话，共 N 轮" / "上次总结(00y总结.md)后新增内容，第 M~N 轮"）

## 2. 对话核心内容概括
（此处输出精简的结构化总结，分点梳理：用户诉求、AI 解答、关键信息、待办事项）

### 用户主要诉求
- [诉求1]
- [诉求2]

### AI 核心操作/回答
- [操作1]
- [操作2]

### 重要发现与结论
- [发现1]

### 产生的文件 / 代码
- [文件1]
- [文件2]

## 3. 关键信息提取
- **用户核心需求**：
- **核心操作/方案**：
- **遗留问题 / 待跟进**：
```

---

## 三、Git 自动提交推送规则（⚠️ 强制完整流程）

远程仓库固定地址：`git@github.com:Zane42186/AI-Soft-Test.git`

### 执行步骤（必须严格按序执行，任意步骤失败即终止并报错）：

```
步骤1 — 前置校验：
  - 校验本地仓库目录存在
  - 校验 git 命令可用（which git）
  - 校验 SSH 密钥可连通 GitHub（ssh -T git@github.com，允许非零退出码仅验证连通性）

步骤2 — 拉取远程最新：
  - 进入仓库根目录
  - 执行 git pull origin main（或 master，以实际默认分支为准）
  - 若 pull 失败/有冲突，输出完整错误日志，终止流程并提示人工处理

步骤3 — 暂存文件（⚠️ 暂存仓库根目录所有变更）：
  - 执行 git add . 暂存仓库根目录下所有变更内容（新增、修改、删除）
  - 包含但不限于：Conclusion/00x总结.md、Conclusion/.summary_record.json 以及项目中所有其他有变动的文件
  - 确保整个仓库的工作区变更被完整纳入本次提交

步骤4 — 提交：
  - Commit 信息严格使用：「00x提交」，x 与生成的 md 文件序号一致
  - 示例：生成 001总结.md → commit 信息为 "001提交"

步骤5 — 推送：
  - 执行 git push origin main

步骤6 — 异常处理：
  - 任意 git 步骤报错，输出完整错误日志（stdout + stderr）
  - 终止流程并明确提示用户人工处理
```

---

## 四、Skill 内置持久化状态文件

### 4.1 状态文件位置
- `Conclusion/.summary_record.json`

### 4.2 文件格式
```json
{
  "version": "1.0",
  "currentConversationId": "本次对话的会话ID",
  "lastConversationId": "上次总结的对话ID",
  "lastMessageIndex": 0,
  "maxFileNumber": 0,
  "lastSummaryFile": "00x总结.md",
  "lastSummaryTime": "2026-06-17T12:00:00",
  "totalSummariesInConversation": 0
}
```

### 4.3 更新时机
- 每次总结完成、md 文件写入成功后，立即更新 `.summary_record.json`
- 更新逻辑：
  - `lastConversationId` ← `currentConversationId`
  - `currentConversationId` ← 当前会话的实际 ID
  - `lastMessageIndex` ← 当前已截取到的最后一条消息索引
  - `maxFileNumber` ← 本次生成的文件序号
  - `lastSummaryFile` ← 本次生成的文件名
  - `lastSummaryTime` ← 当前时间 ISO 8601 格式
  - `totalSummariesInConversation` += 1

---

## 五、输出反馈格式

每次完整执行完【生成 md 文件 + Git 推送】后，向用户输出简洁回执：

```
✅ 已完成 {全量/增量} 总结
📄 文件：Conclusion/{文件名}.md
📤 已提交推送至远程仓库 {仓库地址}
💬 提交备注：{序号}提交
📊 本次总结覆盖 {N} 轮对话
```

---

## 六、禁止行为（红线）

1. ❌ **禁止修改** `Conclusion/` 目录内已有的历史 md 文件
2. ❌ **禁止跳过** `git pull` 直接提交推送
3. ❌ **禁止**文件名不按三位补零格式（如 `1总结.md`、`01总结.md`）
4. ❌ **禁止**跨对话混淆分段标记（新对话必须全量总结，不得沿用旧对话的 messageIndex）
5. ❌ **禁止**总结时遗漏用户关键需求、操作步骤
6. ❌ **禁止**在 Git 操作失败后仍然标记总结"成功完成"

---

## 执行流程图

```
用户输入「总结当前内容」
  │
  ├─ 去重检查：30s 内重复？→ 忽略
  │
  ├─ 读取 Conclusion/.summary_record.json
  │     │
  │     ├─ 不存在 → 全量模式
  │     ├─ 新对话ID → 全量模式
  │     └─ 同对话ID → 增量模式（从 lastMessageIndex+1 开始截取）
  │
  ├─ 扫描 Conclusion/ 目录，计算新文件序号
  │
  ├─ 生成 00x总结.md（严格遵循模板）
  │
  ├─ 更新 .summary_record.json
  │
  ├─ Git 流程：pull → add . (所有变更) → commit("00x提交") → push
  │     │
  │     └─ 失败 → 输出错误日志，终止
  │
  └─ 输出成功回执
```
