# 第六天：Linux 基础入门（一）— 文件系统与基本操作

> 📅 学习日期：第六天 | 🎯 模块：Linux 基础 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：为什么测试工程师必须学 Linux？

功能测试模块结束后，你可能会问："我做测试，为什么要学 Linux？"

答案很简单：**绝大多数软件都运行在 Linux 服务器上。** 作为测试工程师，你迟早会遇到这些场景：

| 场景 | 需要的 Linux 技能 |
|------|-------------------|
| 🔍 **查日志定位 Bug** | `tail -f`、`grep`、`less` |
| 📊 **看服务器状态** | `top`、`free`、`df` |
| 🚀 **部署测试环境** | `cd`、`cp`、`tar`、`chmod` |
| 🐧 **执行自动化脚本** | Shell 脚本基础 |
| 🔗 **测试接口连通性** | `curl`、`ping`、`telnet` |

> 🎯 **一句话**：不会 Linux 的测试工程师，就像不会用听诊器的医生——你能看到表面症状，但看不到系统内部的真实状态。

---

## 一、Linux 是什么？

### 1.1 一句话定义

**Linux** 是一个开源的类 Unix 操作系统内核，加上外围工具和应用程序后构成完整的操作系统。

通俗来说：Linux 就是**服务器端的操作系统**——就像 Windows 是你个人电脑的操作系统，Linux 是服务器世界的"Windows"。

### 1.2 Linux 发行版

由于 Linux 开源，不同组织在它上面加了自己的"包装"，形成了不同的发行版：

| 发行版 | 特点 | 常见使用场景 |
|--------|------|-------------|
| **CentOS** / **Red Hat** | 稳定、企业级 | 传统企业服务器 |
| **Ubuntu** | 用户友好、资料多 | 个人学习、云服务器 |
| **Debian** | 极度稳定 | 对稳定性要求极高的场景 |
| **Alpine** | 极小体积（~5MB） | Docker 容器镜像 |

> 📌 对于测试工程师来说，**Ubuntu 或 CentOS** 最常遇到。它们的命令 95% 相同，区别主要在于包管理器（`yum` vs `apt`）。

### 1.3 如何获取 Linux 环境？

| 方式 | 难度 | 推荐度 |
|------|------|--------|
| Windows WSL2（推荐） | ⭐ 简单 | ⭐⭐⭐⭐⭐ |
| 虚拟机（VMware/VirtualBox） | ⭐⭐ 中等 | ⭐⭐⭐ |
| 云服务器（阿里云/腾讯云） | ⭐⭐ 中等 | ⭐⭐⭐⭐ |
| Docker 容器 | ⭐⭐ 中等 | ⭐⭐⭐ |

> 💡 **对于 Windows 用户**：强烈建议安装 WSL2（Windows Subsystem for Linux）。只需一条命令即可在 Windows 上拥有完整的 Linux 环境。本教程假设你已有可用的 Linux 终端。

---

## 二、第一次打开 Linux 终端

### 2.1 终端长什么样？

```
zane@server:~$ 
```

解读这段提示符：

| 部分 | 含义 |
|------|------|
| `zane` | 当前登录的用户名 |
| `@server` | 当前所在的服务器/主机名 |
| `~` | 当前所在的目录（`~` 表示当前用户的家目录，类似 Windows 的 `C:\Users\zane`） |
| `$` | 提示符结尾（`$` = 普通用户，`#` = root 超级管理员） |

### 2.2 最重要的命令：`man`（帮助手册）

在学任何命令之前，先学会**自己查帮助**：

```bash
man ls          # 查看 ls 命令的完整手册
ls --help       # 查看 ls 命令的简要帮助（大多数命令支持）
man -k keyword  # 搜索与 keyword 相关的命令
```

> 🎯 **授人以渔**：不用背所有命令参数。遇到新命令先 `--help`，记不清细节用 `man`。

---

## 三、文件系统操作命令

Linux 系统的一切都是文件——目录是文件、配置是文件、进程信息也是文件。掌握文件操作是 Linux 的第一课。

### 3.1 核心命令速查

| 命令 | 全称 | 用途 | 测试场景举例 |
|------|------|------|-------------|
| `pwd` | Print Working Directory | 显示当前所在目录 | "我现在在哪个目录？" |
| `ls` | List | 列出目录内容 | "这个目录下有哪些日志文件？" |
| `cd` | Change Directory | 切换目录 | "进入日志文件夹" |
| `mkdir` | Make Directory | 创建目录 | "创建测试数据存放目录" |
| `rm` | Remove | 删除文件/目录 | "清理测试产生的临时文件" |
| `cp` | Copy | 复制文件/目录 | "备份测试配置文件" |
| `mv` | Move | 移动/重命名文件 | "重命名测试报告" |
| `tree` | Tree | 树状显示目录结构 | "导出项目的目录结构" |

### 3.2 `pwd` — 我在哪里？

```bash
pwd
# 输出：/home/zane/projects
```

> 📌 就像一个没有 GUI 的"地址栏"，告诉你当前在文件系统中的位置。

### 3.3 `ls` — 看看有什么？

```bash
ls              # 简单列出当前目录的文件和子目录
ls -l           # 详细列表（权限、大小、时间）
ls -lh          # 详细列表 + 人类可读的文件大小（K/M/G）
ls -la          # 详细列表 + 显示隐藏文件（.开头的文件）
ls -ltr         # 按修改时间排序，最新的在最下面
```

#### 实战：看懂 `ls -l` 的输出

```bash
$ ls -l
-rw-r--r-- 1 zane dev  2048 Jun 20 14:30 test.log
drwxr-xr-x 2 zane dev  4096 Jun 19 09:15 scripts/
```

逐个字段解释：

```
-rw-r--r--  1  zane  dev   2048  Jun 20 14:30  test.log
  │          │    │     │     │        │          │
  │          │    │     │     │        │          └── 文件名
  │          │    │     │     │        └── 最后修改时间
  │          │    │     │     └── 文件大小（字节）
  │          │    │     └── 所属组
  │          │    └── 所有者
  │          └── 硬链接数
  └── 文件类型+权限（后面详解）
```

> 📌 文件类型：`-` = 普通文件，`d` = 目录，`l` = 软链接（快捷方式）

#### 常用组合技

```bash
ls -lh | grep ".log"     # 列出所有 .log 文件，含人类可读的大小
ls -ltr | tail -5        # 列出最近修改的 5 个文件
ls -l | wc -l            # 统计当前目录下有多少个文件/目录
```

### 3.4 `cd` — 我要去哪里？

```bash
cd /var/log          # 进入 /var/log 目录（绝对路径）
cd logs              # 进入当前目录下的 logs 子目录（相对路径）
cd ..                # 返回上一级目录
cd ~                 # 回到自己的家目录
cd -                 # 回到上次所在的目录（非常实用！）
cd                   # 不带参数默认为回到家目录
```

#### 绝对路径 vs 相对路径

```
绝对路径：从根目录 / 开始写，如 /home/zane/projects/test
相对路径：从当前目录开始写，如 ./test（./ 表示当前目录）
```

| 路径类型 | 类比 | 示例 |
|----------|------|------|
| **绝对路径** | GPS 坐标（全球唯一） | `/var/log/app/error.log` |
| **相对路径** | "往前走 200 米再右转"（相对当前位置） | `../logs/error.log` |

> 🎯 **测试场景**：当你通过 SSH 登录服务器排查问题，第一步就是 `cd /var/log` 或 `cd /opt/app/logs` 找到日志目录。

### 3.5 `mkdir` — 创建目录

```bash
mkdir test_data             # 创建一个目录
mkdir -p a/b/c/d            # 递归创建多级目录（非常实用！）
mkdir -p /tmp/test/{data,logs,reports}  # 批量创建多个目录（{ } 展开）
```

> 🎯 **测试场景**：`mkdir -p /tmp/test/{input,output,expected}` — 为自动化测试创建输入/输出/预期结果的目录结构。

### 3.6 `rm` — 小心删除！

```bash
rm file.txt                 # 删除文件
rm -i file.txt              # 删除前确认（y/n）
rm -r test_dir/             # 递归删除目录及其中所有内容
rm -rf test_dir/            # 强制递归删除，不询问（⚠️ 危险！）
```

> ⚠️ **红线警告**：`rm -rf /` 会删除整个系统！`rm -rf *` 会删除当前目录下所有内容！删除前请**先 `ls` 确认目录内容**，再执行删除。

> 💡 **安全习惯**：先用 `ls` 列出要删除的文件确认无误，再替换 `ls` 为 `rm`。或者始终使用 `rm -i` 开启确认模式。

### 3.7 `cp` — 复制

```bash
cp source.txt dest.txt        # 复制文件
cp -r source_dir/ dest_dir/   # 递归复制整个目录
cp -p file1 file2             # 复制并保留文件属性（权限、时间戳）
cp -a source_dir/ dest_dir/   # 归档复制（保留所有属性+软链接）
```

> 🎯 **测试场景**：`cp /opt/app/config.yml /opt/app/config.yml.bak` — 修改配置前先备份！

### 3.8 `mv` — 移动/重命名

```bash
mv old_name.txt new_name.txt    # 重命名
mv file.txt /tmp/               # 移动到 /tmp 目录
mv -i source dest               # 移动前确认（防止覆盖）	 	
```

> 📌 Linux 中没有"重命名"这个单独的命令，`mv` 同时负责移动和重命名。

### 3.9 `tree` — 可视化目录结构

```bash
tree                  # 树状显示当前目录
tree -L 2             # 只显示 2 层深度
tree -d               # 只显示目录，不显示文件
```

输出示例：
```
.
├── logs/
│   ├── app.log
│   └── error.log
├── config/
│   └── app.yml
└── scripts/
    └── deploy.sh
```

> 📌 如果提示 `tree: command not found`，需要安装：`sudo apt install tree`（Ubuntu）或 `sudo yum install tree`（CentOS）。

---

## 四、文件内容查看命令

测试工程师 80% 的 Linux 操作都是在**看日志**。以下命令从简单到复杂排列：

| 命令 | 用途 | 常用场景 |
|------|------|----------|
| `cat` | 显示完整文件内容 | 看小文件、配置文件 |
| `less` | 分页浏览文件（可上下翻页） | 浏览大文件、大日志 |
| `head` | 看文件前 N 行 | 快速确认日志格式 |
| `tail` | 看文件后 N 行 | 查最新日志、**实时跟踪日志** |

### 4.1 `cat` — 一次性显示全部

```bash
cat app.log                    # 显示全部内容
cat -n app.log                 # 显示行号
cat file1.log file2.log > merged.log  # 合并多个文件
```

> ⚠️ `cat` 适合小文件。对大日志文件（几百 MB）用 `cat` 会让终端刷屏很久——这时用 `less`。

### 4.2 `less` — 分页浏览（重点掌握）

```bash
less app.log          # 打开文件，进入分页浏览模式
```

进入 `less` 后的操作：

| 按键 | 功能 |
|------|------|
| `↑` / `↓` 或 `j` / `k` | 上/下滚动一行 |
| `Space` 或 `PgDn` | 向下翻一页 |
| `b` 或 `PgUp` | 向上翻一页 |
| `g` | 跳到文件开头 |
| `G`（Shift+g） | 跳到文件末尾 |
| `/关键词` | 向下搜索关键词 |
| `?关键词` | 向上搜索关键词 |
| `n` | 跳到下一个匹配项 |
| `N` | 跳到上一个匹配项 |
| `q` | 退出 |

> 🎯 **测试场景**：`less /var/log/app/error.log` → 按 `G` 跳到末尾 → 按 `?ERROR` 向上搜索最近的错误 → 按 `n` 逐个查看。

### 4.3 `head` — 看开头

```bash
head app.log               # 显示前 10 行（默认）
head -n 20 app.log         # 显示前 20 行
head -n 100 app.log | grep "ERROR"  # 前 100 行中搜 ERROR
```

### 4.4 `tail` — 看末尾（⭐ 最重要）

```bash
tail app.log               # 显示最后 10 行（默认）
tail -n 50 app.log         # 显示最后 50 行
tail -f app.log            # 🔥 实时跟踪日志（Ctrl+C 退出）
tail -f app.log | grep "ERROR"  # 实时跟踪并只显示含 ERROR 的行
```

> 🎯 **这是测试工程师最常用的命令组合**：`tail -f app.log`
>
> **场景**：开发说"我已经部署了，你测一下"。你在另一个终端执行 `tail -f app.log` 实时看日志，然后操作 App，所有请求日志、错误信息实时滚出——Bug 无处遁形。

---

## 五、文件权限基础（一）

### 5.1 为什么需要权限？

Linux 是多用户系统，同一台服务器可能有多个用户同时使用。**权限控制谁可以读、写、执行哪些文件。**

### 5.2 理解 `ls -l` 中的权限字段

回顾之前 `ls -l` 的输出：

```
-rw-r--r-- 1 zane dev 2048 Jun 20 14:30 test.log
 └─────┬──────┘
       │
    权限位（10 个字符）
```

权限位拆解：

```
  -   rw-   r--   r--
  │    │     │     │
  │    │     │     └── 其他人的权限（other）
  │    │     └── 所属组的权限（group）
  │    └── 所有者的权限（owner）
  └── 文件类型（d=目录, -=文件, l=链接）
```

每种权限：

| 字符 | 全称 | 对文件 | 对目录 |
|------|------|--------|--------|
| **r** | Read（读） | 可以查看文件内容 | 可以列出目录内容（ls） |
| **w** | Write（写） | 可以修改文件内容 | 可以在目录中创建/删除文件 |
| **x** | eXecute（执行） | 可以执行该文件（脚本/程序） | 可以进入该目录（cd） |

### 5.3 实战解读

```
-rw-r--r--   → 普通文件，所有者可读写，组用户和其他人只读
-rwxr-xr-x   → 普通文件，所有者可读写执行，组和其他人可读执行（常见于脚本）
drwxr-x---   → 目录，只有所有者和组可以进入和查看，其他人完全无权限
```

> 📌 今天先了解权限的结构和含义，后面的学习会深入讲解 `chmod` 如何修改权限。

---

## 六、课后练习

### 练习一：命令匹配（10 分钟）

将以下测试场景与最合适的 Linux 命令配对：

| 场景 | 你的答案 |
|------|----------|
| 1. 想知道当前在哪个目录 | pwd |
| 2. 查看 /var/log 目录下有哪些 `.log` 文件，显示文件大小 | ll /var/log/*.log |
| 3. 进入 `/opt/app/config` 目录 | cd /opt/app/config |
| 4. 为测试创建 `/tmp/test/data` 和 `/tmp/test/logs` 两个目录 | mkdir -p /tmp/test/{data,logs} |
| 5. 复制配置文件并备份为 `.bak` | cp file.txt file.txt.bak |
| 6. 实时监控应用日志，看有没有新错误 | tail -f app.log \| grep "error" |
| 7. 翻看一个 500MB 的大日志文件，搜索 "NullPointerException" | less app.log |
| 8. 查看配置文件的前 20 行 | head -n 20 config.yml |

可选命令：`cd`, `pwd`, `ls -lh *.log`, `mkdir -p /tmp/test/{data,logs}`, `cp config.yml config.yml.bak`, `tail -f app.log`, `less app.log`, `head -n 20 config.yml`

---

### 练习二：权限解读（10 分钟）

请解读以下 `ls -l` 输出中每个文件的权限含义：

```
$ ls -l
drwxr-xr-x 2 zane dev 4096 Jun 20 10:00 scripts
-rwxr--r-- 1 zane dev  128 Jun 20 09:30 deploy.sh
-rw------- 1 zane dev   64 Jun 20 08:00 secret.key
drwxr-x--- 2 zane dev 4096 Jun 19 16:00 logs
```

| 文件 | 类型 | 所有者权限 | 组权限 | 其他人权限 | 含义 |
|------|------|-----------|--------|-----------|------|
| scripts | 目录 | 读-写-执行 | 读-执行 | 执行 | 目录，所有者可以进入和查看并创建和删除目录下文件，组成员可以查看目录下文件和进入目录，其他人只可进入 |
| deploy.sh | 文件 | 读写执行 | 读 | 读 | 文件，所有者可查看修改执行文件，其他人和组成员只可读 |
| secret.key | 文件 | 读写 | 无 | 无 | 文件，所有者可查看和修改文件，其他人无权限 |
| logs | 目录 | 读写执行 | 读执行 | 无 | 目录，所有者可查看和进入目录，并可以在目录下创建删除目录和文件。组成员可查看和进入目录，其他人无权限 |

---

### 练习三：模拟排查日志（15 分钟）

> 假设你通过 SSH 登录了一台测试服务器，需要排查一个应用错误。

请按步骤写出你将使用什么命令完成以下操作：

1. **进入日志目录**：通常应用日志在 `/var/log/myapp/`，请写出进入该目录的命令。
   cd /var/log/myapp/		
2. **查看日志文件列表**：列出该目录下所有文件，按修改时间排序，显示文件大小。
   ls -ltr
3. **找到最新的错误日志**：日志文件名格式为 `app-YYYY-MM-DD.log`，今天的日志文件名是什么？写出命令查看该文件最后 50 行。
   app-2026-07-08.log tail -n 50 app-2026-07-08.log
4. **实时监控**：用一条命令实时跟踪日志输出，并只显示包含 "ERROR" 的行。
   tail -f app.log | grep "ERROR"
5. **搜索历史错误**：在今天的日志文件中搜索所有包含 "Timeout" 的行，并统计出现了多少次。
   grep -c "Timeout" app.log

> 💡 提示：第 5 小题可能需要用到下一天才会讲的 `grep` 命令。可以先想想思路，下一天验证。

---

## 📋 第六天自检清单

完成学习后，确认以下知识点：

- [ ] 能说出为什么测试工程师需要学 Linux（至少 3 个场景）
  1. 大部分软件都运行在Liunx服务器下
  2. 查看日志定位bug
  3. 运行自动化脚本
  4. 部署环境
- [ ] 能解释终端提示符 `user@host:~$` 各部分的含义
  1. user 用户名
  2. host 主机名
  3. ~ 当前目录
  4. $ 普通用户
- [ ] 知道如何查看命令的帮助（`--help` 和 `man`）
- [ ] 能用 `pwd`、`ls`、`cd` 完成基本目录导航
- [ ] 能区分绝对路径和相对路径
- [ ] 能用 `mkdir -p` 递归创建目录，用 `{ }` 批量创建
- [ ] 知道 `rm -rf` 的危险性，有删除前确认的习惯
- [ ] 能用 `cp` 备份文件，用 `mv` 重命名/移动文件
- [ ] 能区分 `cat`、`less`、`head`、`tail` 的使用场景
- [ ] 能使用 `tail -f` 实时跟踪日志
- [ ] 能在 `less` 中搜索关键词（`/关键词`）和翻页
- [ ] 能解读 `-rw-r--r--` 格式的权限含义
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第七天：文本处理三剑客（grep / awk / sed）**

这是 Linux 学习中**最重要的三个命令**——掌握了它们，你就能从海量日志中快速提取有用信息。这也是测试面试中 Linux 部分的高频考点。

- `grep`：从文本中搜索特定内容（"日志里所有 ERROR 找出来"）
- `sed`：对文本进行查找替换（"把所有 IP 地址脱敏"）
- `awk`：对结构化文本进行格式化处理（"提取日志第 3 列和第 5 列"）

---

> ✨ **第六天的核心心法**：Linux 不是用来"学"的，是用来"用"的。每个命令都要在终端里敲一遍。今天学完 `tail -f`，明天就找个真实日志试一试。**肌肉记忆比大脑记忆更可靠。**

---

# 第七天：文本处理三剑客 — grep / sed / awk

> 📅 学习日期：第七天 | 🎯 模块：Linux 基础 → 文本处理 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：从"看日志"到"分析日志"

第六天你学会了怎么看日志——`tail -f` 实时跟踪、`less` 分页浏览。但测试工程师不只是"看"日志，更要**从海量日志中提取关键信息**。

比如这些场景：

| 场景 | 你需要什么能力 |
|------|---------------|
| 🔍 "帮我找出今天所有 ERROR 级别的日志" | **搜索过滤** |
| 📊 "统计每个接口的响应时间超过 1s 的次数" | **格式化提取 + 统计** |
| 🛡️ "把日志里的用户手机号中间 4 位打码" | **文本替换** |
| 📋 "提取日志第 1 列（时间）和第 5 列（响应时间），导出为 CSV" | **列提取** |

这三个命令——`grep`、`sed`、`awk`——合称 Linux **"文本处理三剑客"**。掌握了它们，你就能像 Ctrl+F + Excel 透视表一样在终端里处理任何文本。

---

## 一、`grep` — 文本搜索之王

### 1.1 一句话定义

> **`grep`** = Global Regular Expression Print，在文本中搜索匹配的行并输出。

简单说：**grep 就是命令行的 Ctrl+F**，但它强大得多。

### 1.2 基本语法

```bash
grep [选项] "搜索模式" 文件名
```

### 1.3 核心选项速查

| 选项 | 全称/含义 | 用途 | 测试场景举例 |
|------|----------|------|-------------|
| `-i` | ignore case | 忽略大小写 | `grep -i "error" app.log` — 同时匹配 Error/ERROR/error |
| `-v` | invert match | 反向匹配（排除） | `grep -v "DEBUG" app.log` — 排除 DEBUG 日志 |
| `-n` | line number | 显示行号 | `grep -n "ERROR" app.log` — 知道第几行出错 |
| `-c` | count | 统计匹配行数 | `grep -c "Timeout" app.log` — 超时出现了多少次 |
| `-r` | recursive | 递归搜索目录 | `grep -r "Exception" /var/log/` — 在所有日志中搜索 |
| `-E` | extended regex | 扩展正则表达式 | `grep -E "ERROR|FATAL" app.log` — 同时搜多个关键词 |
| `-A N` | after N lines | 显示匹配行后 N 行 | `grep -A 3 "ERROR" app.log` — 看错误上下文 |
| `-B N` | before N lines | 显示匹配行前 N 行 | `grep -B 2 "ERROR" app.log` — 看错误前发生了什么 |
| `-C N` | context N lines | 显示匹配行前后 N 行 | `grep -C 5 "Exception" app.log` — 完整错误上下文 |
| `--color` | highlight | 高亮匹配内容 | `grep --color "ERROR" app.log` — 一眼看到匹配项 |

### 1.4 实战：测试工程师的 `grep` 日常

#### 场景 1：查今天的错误日志

```bash
grep "ERROR" /var/log/myapp/app-2026-07-08.log
```

#### 场景 2：统计今天每种错误分别出现了多少次

```bash
grep "ERROR" app.log | awk '{print $NF}' | sort | uniq -c | sort -rn
#                                           ↑ 后续会讲
# 输出示例：
#   45 NullPointerException
#   12 TimeoutException
#    3 DatabaseConnectionError
```

#### 场景 3：搜索某个时间段内的日志（配合正则）

```bash
grep "2026-07-08 14:.." app.log | grep "ERROR"
# 正则 . 匹配任意字符，14:.. 匹g配 14:00~14:59
```

#### 场景 4：排除干扰信息

```bash
grep "Exception" app.log | grep -v "ExpectedException"
# 找所有异常，但排除测试中刻意触发的 ExpectedException
```

> 🎯 `grep -v` 在测试中非常实用——排除已知的、不关心的噪音，聚焦真正有价值的错误。

### 1.5 `grep` 常用正则符号

| 符号 | 含义 | 示例 |
|------|------|------|
| `.` | 匹配任意一个字符 | `80.` 匹配 8080、8000、80x |
| `*` | 前一个字符出现 0 次或多次 | `ab*c` 匹配 ac、abc、abbc |
| `^` | 行首 | `^ERROR` — 以 ERROR 开头的行 |
| `$` | 行尾 | `error$` — 以 error 结尾的行 |
| `[]` | 匹配括号内任意一个字符 | `[Ee]rror` 匹配 Error 或 error |
| `|` | 或（需加 `-E`） | `ERROR|FATAL` — ERROR 或 FATAL |
| `\b` | 单词边界（需 `-E`） | `\b500\b` — 只匹配独立的 500（不匹配 1500） |

> 📌 正则表达式不需要一次全记住。用到的时候查一下就行。重点记住 `.` `*` `^` `$` `|` 五个就够了。

---

## 二、`sed` — 流式文本编辑器

### 2.1 一句话定义

> **`sed`** = Stream Editor，对文本进行**逐行处理**，可以替换、删除、插入、提取。

简单说：**sed 就是命令行的"查找替换"工具**，但不需要打开文件编辑器。

### 2.2 基本语法

```bash
sed [选项] '操作命令' 文件名
```

### 2.3 核心用法

#### 用法 1：查找替换（最常用）

```bash
sed 's/旧内容/新内容/g' 文件名
#    s = substitute（替换）
#    g = global（全局，每行替换所有匹配项；不加 g 只替换每行第一个）
```

**实战示例**：

```bash
# 把日志中的 IP 地址脱敏
echo "User 192.168.1.100 login success" | sed 's/192.168.1.100/[REDACTED]/g'
# 输出：User [REDACTED] login success

# 把日志中所有手机号中间 4 位打码
echo "用户 18912345678 下单成功" | sed -E 's/([0-9]{3})[0-9]{4}([0-9]{4})/\1****\2/g'
# 输出：用户 189****5678 下单成功
```

> 🎯 **测试场景**：在给开发贴日志之前，用 `sed` 把用户敏感信息脱敏——这是测试工程师的职业素养。

#### 用法 2：删除特定行

```bash
sed '/匹配模式/d' 文件名     # d = delete
# 示例：删除所有空行和注释行
sed '/^$/d; /^#/d' config.yml
```

#### 用法 3：只打印特定行

```bash
sed -n '10,20p' 文件名       # 只输出第 10~20 行
sed -n '/ERROR/p' 文件名     # 只输出包含 ERROR 的行（类似 grep）
```

#### 用法 4：原地修改文件（⚠️ 谨慎）

```bash
sed -i 's/旧/新/g' 文件名    # -i = in-place，直接修改文件
sed -i.bak 's/旧/新/g' 文件名 # 修改前备份为 .bak（推荐！）
```

> ⚠️ `sed -i` 会直接修改文件，建议始终加 `.bak` 后缀做备份。

### 2.4 `sed` 常用选项

| 选项 | 含义 | 示例 |
|------|------|------|
| `-n` | 静默模式，只输出明确要求的内容 | `sed -n '5p' file` — 只打印第 5 行 |
| `-i` | 原地修改文件 | `sed -i 's/old/new/g' file` |
| `-i.bak` | 原地修改前先备份 | `sed -i.bak 's/old/new/g' file` |
| `-E` | 支持扩展正则表达式 | `sed -E 's/[0-9]{3}/xxx/g' file` |
| `-e` | 执行多个命令 | `sed -e 's/a/A/g' -e 's/b/B/g' file` |

---

## 三、`awk` — 结构化文本处理引擎

### 3.1 一句话定义

> **`awk`** = 以列为单位处理文本，可以对每一列做计算、过滤、格式化。

简单说：**awk 就是命令行的 Excel**——它把每行文本按列拆分，你可以对任意列做操作。

### 3.2 核心概念：自动按列拆分

```bash
$ echo "2026-07-08 14:30:15 ERROR NullPointerException /api/login"
2026-07-08 14:30:15 ERROR NullPointerException /api/login
 │            │     │     │                     │
 $1           $2    $3    $4                    $5       ← awk 列号
```

`awk` 默认以**空格/Tab** 为分隔符，把每行拆成若干列：`$1` 第一列、`$2` 第二列……`$0` 表示整行。

### 3.3 基本语法

```bash
awk [选项] '条件 { 动作 }' 文件名
```

最常见的格式：

```bash
awk '{print $1, $3}' 文件名    # 打印第 1 列和第 3 列
```

### 3.4 实战案例

#### 案例 1：提取日志中的特定列

```bash
# 日志格式：时间 级别 消息
# 只提取时间和级别
awk '{print $1, $2, $3}' app.log
```

#### 案例 2：加条件过滤

```bash
# 只输出 ERROR 级别日志的时间和消息内容
awk '$3 == "ERROR" {print $1, $2, $4, $5}' app.log
#     ↑ 条件                  ↑ 动作
```

#### 案例 3：统计计算

```bash
# 统计所有订单金额的总和（假设第 5 列是金额）
awk '{sum += $5} END {print "总金额:", sum}' orders.txt

# 统计所有接口响应时间的平均值
awk '{total += $3; count++} END {print "平均响应时间:", total/count "ms"}' access.log
```

#### 案例 4：按自定义分隔符处理 CSV

```bash
# CSV 文件用逗号分隔
awk -F ',' '{print $1, $3}' data.csv
#    -F = Field separator（列分隔符）
```

### 3.5 `awk` 核心要素速查

| 要素 | 含义 | 示例 |
|------|------|------|
| `$1`, `$2`, `$3`... | 第 N 列 | `print $1` — 打印第一列 |
| `$0` | 整行 | `print $0` — 打印整行 |
| `$NF` | 最后一列（Number of Fields） | `print $NF` — 打印最后一列 |
| `NR` | 当前行号（Number of Records） | `NR==1` — 第一行（通常是表头） |
| `-F` | 指定列分隔符 | `-F ','` — 按逗号分隔 |
| `BEGIN {}` | 处理前执行一次 | `BEGIN {print "---报告开始---"}` |
| `END {}` | 处理后执行一次 | `END {print "---报告结束---"}` |

---

## 四、三剑客组合技（⭐ 核心价值）

单个命令是工具，**管道组合才是武器**。

### 4.1 管道回顾

```bash
命令A | 命令B | 命令C
#       ↑ 把 A 的输出作为 B 的输入
```

### 4.2 测试工程师常用组合

#### 组合 1：搜索 → 提取 → 统计

```bash
# 找出所有 ERROR，提取异常类型，统计每种异常出现次数
grep "ERROR" app.log | awk '{print $NF}' | sort | uniq -c | sort -rn
#                   └──提取最后一列   └──排序  └──去重统计  └──按次数降序
```

#### 组合 2：搜索 → 提取列 → 计算

```bash
# 统计 ERROR 日志中所有接口的平均响应时间（假设第 6 列是耗时）
grep "ERROR" app.log | awk '{sum+=$6; count++} END {print "平均耗时:", sum/count "ms"}'
```

#### 组合 3：实时监控 + 过滤 + 脱敏

```bash
# 实时看日志，过滤 ERROR，手机号打码
tail -f app.log | grep "ERROR" | sed -E 's/[0-9]{11}/[手机号已脱敏]/g'
```

#### 组合 4：快速生成测试报告

```bash
# 从 JMeter 结果日志中提取成功率
grep -c "false" results.jtl | awk '{print "失败数:", $1}'
grep -c "true" results.jtl | awk '{print "成功数:", $1}'
```

> 🎯 **一句话**：`grep` 负责找、`sed` 负责改、`awk` 负责算。管道 `|` 把它们串成一条流水线。

---

## 五、三剑客对比总结

| 维度 | `grep` | `sed` | `awk` |
|------|--------|-------|-------|
| **最适合** | 搜索/过滤行 | 替换/删除/插入文本 | 列提取/统计/格式化输出 |
| **思维模型** | "哪些行包含这个？" | "把这些替换成那些" | "取第 3 列，算平均值" |
| **类比** | Ctrl+F | 查找替换 | Excel 透视表 |
| **测试场景** | 找错误日志 | 脱敏敏感信息 | 统计接口响应时间 |
| **难度** | ⭐ 入门 | ⭐⭐ 中等 | ⭐⭐⭐ 进阶 |
| **学习优先级** | 🔴 必学（Day 7） | 🟡 必学（Day 7） | 🟡 必学（Day 7） |

---

## 六、课后练习

### 练习一：grep 实战（15 分钟）

假设有以下日志文件 `app.log`，请写出对应的命令：

```
2026-07-08 10:00:01 INFO  UserService User admin login success
2026-07-08 10:01:15 ERROR UserService NullPointerException at login()
2026-07-08 10:02:30 DEBUG PaymentService processing order #12345
2026-07-08 10:03:45 ERROR PaymentService TimeoutException: payment gateway
2026-07-08 10:04:00 INFO  UserService User guest logout
2026-07-08 10:05:22 ERROR UserService NullPointerException at login()
2026-07-08 10:06:30 WARN  PaymentService slow response: 3500ms
2026-07-08 10:07:00 ERROR DatabaseService Connection refused
```

| 需求 | 你的命令 |
|------|----------|
| 1. 找出所有 ERROR 级别的日志 | grep "ERROR" app.log |
| 2. 找出所有 ERROR，并显示行号 | grep -n "ERROR" app.log |
| 3. 统计 ERROR 出现了多少次 | grep -c "ERROR" app.log |
| 4. 找出包含 "PaymentService" 的所有行（不区分大小写） | grep -i "PaymentService" app.log |
| 5. 找出所有 ERROR 日志及其前 1 行（看错误触发条件） | grep -B 1 "ERROR" app.log |
| 6. 找出所有行，但排除 DEBUG 级别的日志 | grep -v "DEBUG" app.log |
| 7. 统计每种异常类型（NullPointerException/TimeoutException等）各出现几次 | grep "ERROR" app.log \| awk '{print $4}' \| sort \| uniq -c \| sort -rn |
| **日志结构：时间 时分 级别 异常名 接口...** | 管道符传递的是内容而不是拼接命令 |

`sort`

````
把所有异常名称按字母顺序排序，让相同异常挨在一起，为 `uniq` 统计做准备。
````

`uniq -c`

```
-c：count，统计连续重复行的出现次数
输出格式：次数 异常名称
例：35 NullPointerException
```

`sort -rn`

```
-r：reverse 倒序（从大到小）
-n：numeric 按数字大小排序（不是按字符）
作用：把异常按报错次数从多到少排列，高频错误放最前面。
```




---





### 练习二：sed 实战（10 分钟）

| 需求 | 你的命令 |
|------|----------|
| 1. 把日志中所有 "ERROR" 替换为 "**ERROR**"（不改文件，只输出） | sed 's/ERROR/**ERROR**/g' app.log |
| 2. 把 app.log 中所有 IP `192.168.1.100` 替换为 `[内网IP]` | sed 's/192.168.1.100/[内网IP]/g' app.log |
| 3. 删除 app.log 中所有空行 | sed '/^$/d' app.log |
| 4. 只输出 app.log 的第 5~10 行 | sed -n '5,10p' app.log |
| 5. 把 config.yml 中的所有 "localhost" 替换为 "192.168.1.50"，并备份原文件 | sed -i.bak 's/localhost/192.168.1.50/g' config.yml |

---

### 练习三：awk 实战（15 分钟）

使用练习一的同一份日志数据：

| 需求 | 你的命令 |
|------|----------|
| 1. 只输出每行日志的第 1 列（日期）和第 4 列（服务名） | awk '{print $1,$4}' app.log |
| 2. 只输出 ERROR 级别日志的日期和消息内容（第 5 列起） | awk '$3 == "ERROR" {print  $1,$2,$4,$5}' app.log |
| 3. 统计日志总行数 | awk '{count++} END {print "总行数:" , count}' app.log |
| 4. 输出时在每行前面加上行号（格式：`[行号] 原始内容`） | awk  ' {[print "["NR"]" , $0}' app.log |
| 5. 输出所有 "PaymentService" 相关日志的最后一列 | awk '$4 == "PaymentService" {print $NF}' app.log |

---

### 练习四：组合技实战（10 分钟）

| 需求 | 你的命令 |
|------|----------|
| 1. 找出所有 ERROR，提取服务名（第 4 列），统计每个服务的 ERROR 次数 | grep "ERROR" app.log \| awk '{print $4}' \| sort \| uniq -c \| sort -rns |
| 2. 实时监控日志，只显示包含 ERROR 或 FATAL 的行 | tail -f app.log \| grep "ERROR\|FATAL" |
| 3. 找出所有包含 "Exception" 的行，只显示日期时间（第 1、2 列）和异常名（最后一列）| grep "Exception" app.log \| awk '{print $1,$2,$NF}' |

---

## 📋 第七天自检清单

完成学习后，确认以下知识点：

- [ ] 能用自己的话说出三剑客各自的定位（grep=搜索、sed=替换、awk=列处理）
  grep 用于查找内容，sed用于修改内容，awk用于输出特定行和计算内容
- [ ] 能使用 `grep -i`、`-v`、`-n`、`-c`、`-E` 完成常见日志搜索任务
  1. grep -i 忽略大小写
  2. -v 反向输出
  3. -n 显示行号
  4. -c统计行数
  5. -E 扩展正则表达式
- [ ] 能使用 `grep -A/-B/-C` 查看匹配行的上下文
  1. -A N （after） 显示匹配行下文N行
  2. -B N  显示匹配行前文N行
  3. -C N 显示上下文N行
- [ ] 能使用 `sed 's/旧/新/g'` 完成文本替换
- [ ] 知道 `sed -i.bak` 可以原地修改并备份
- [ ] 理解 `awk` 按列处理的核心思想（`$1`、`$2`、`$NF`）
- [ ] 能使用 `awk '{print $1, $3}'` 提取指定列
- [ ] 能使用 `awk` 做简单的统计计算（sum/count/avg）
- [ ] 能用管道 `|` 将 grep/sed/awk 组合使用
- [ ] 能独立写出"搜索 → 提取 → 统计"的完整管道命令
- [ ] 完成四道课后练习

---

## 🔜 下一天预告

**第八天：文件权限深入 + 进程管理 + 网络工具**

第七天掌握了文本处理三剑客后，第八天将深入：
- `chmod` / `chown` — 权限管理的实际使用（755、644 数字表示法）
- `ps` / `top` / `kill` — 进程管理（查应用是否在运行、杀死僵尸进程）
- `ping` / `curl` / `netstat` — 网络工具（测试接口连通性、查看端口占用）

这是测试工程师上服务器排查"服务为什么挂了"的必备技能包。

---

> ✨ **第七天的核心心法**：不要试图一次记住所有参数。记住 **`grep` 找、`sed` 改、`awk` 算** 九个字，具体用法碰到了 `--help` 就行。三剑客的真正威力不在单个命令，而在**管道组合**——就像乐高积木，单个只是一块塑料，拼在一起才是一座城堡。

---

# 第八天：权限深入 + 进程管理 + 网络工具

> 🎯 **Day 8 目标**：掌握测试工程师上服务器排查"服务为什么挂了"的三大必备技能包——**权限**（为什么脚本执行不了）、**进程**（应用还在不在运行）、**网络**（端口通不通）。

---

## 第一部分：文件权限深入

### 1.1 复习：rwx 权限模型

第六天我们学过，`ls -l` 输出的权限字符串长这样：

```
-rwxr-xr--  1 user group  4096 Jul 11 10:00 script.sh
│└─┬─┘└─┬─┘└─┬─┘
│  │    │    └── 其他人(r--)：只能读
│  │    └─────── 所属组(r-x)：读+执行
│  └──────────── 所有者(rwx)：读+写+执行
└─────────────── 文件类型(- = 普通文件, d = 目录, l = 软链接)
```

三个权限位永远按 **r → w → x** 顺序排列，有权限显示字母，无权限显示 `-`。

### 1.2 数字表示法（八进制权限）

这是今天的重点——**数字表示法是生产环境的标准语言**，面试必考。

#### 核心规则：r=4, w=2, x=1

| 权限组合 | 二进制思维 | 数字值 | 含义 |
|----------|-----------|--------|------|
| `---` | 0+0+0 | **0** | 无任何权限 |
| `--x` | 0+0+1 | **1** | 仅执行 |
| `-w-` | 0+2+0 | **2** | 仅写入 |
| `-wx` | 0+2+1 | **3** | 写+执行 |
| `r--` | 4+0+0 | **4** | 仅读取 |
| `r-x` | 4+0+1 | **5** | 读+执行 |
| `rw-` | 4+2+0 | **6** | 读+写 |
| `rwx` | 4+2+1 | **7** | 全部权限 |

> 🧠 **记忆口诀**：读=4、写=2、执行=1。想不出某个权限是多少时，在心里做加法——例如 `r-x` = 读(4) + 执行(1) = 5。

#### 三位数字分别代表：所有者 | 所属组 | 其他人

```bash
# 例：chmod 755 script.sh
#      │└┤└┤
#      │ │ └── 其他人 = 5 = r-x（读+执行）
#      │ └──── 所属组 = 5 = r-x（读+执行）
#      └────── 所有者 = 7 = rwx（全部权限）
```

### 1.3 生产环境最常见的四种权限

| 数字 | 权限字符串 | 适用场景 | 为什么 |
|------|-----------|----------|--------|
| **755** | `rwxr-xr-x` | 目录、可执行脚本 | 自己可以改，别人只能看+跑 |
| **644** | `rw-r--r--` | 普通配置文件 | 自己可改，别人只能看 |
| **700** | `rwx------` | 私密脚本/SSH 密钥目录 | 只允许自己访问 |
| **600** | `rw-------` | SSH 私钥、敏感配置文件 | 只允许自己读写 |

> ⚠️ **生产红线**：**永远不要用 `chmod 777`**！777 = 所有人都能读写执行，等于把服务器大门敞开。如果网上教程让你 `chmod 777` 解决权限问题——它在教你走捷径，不是正确方案。

### 1.4 chmod 实战

```bash
# ===== 数字模式（推荐，一步到位） =====
chmod 755 script.sh          # 设为 rwxr-xr-x
chmod 644 config.ini         # 设为 rw-r--r--
chmod 600 ~/.ssh/id_rsa      # 私钥必须 600，否则 SSH 拒绝连接

# ===== 符号模式（精细调整） =====
chmod u+x script.sh          # 给所有者(u)加执行权限(+x)
chmod go-w file.txt          # 去掉组(g)和其他人(o)的写权限(-w)
chmod a+r file.txt           # 所有人(a)加读权限(+r)
chmod o= config.ini          # 清空其他人的所有权限

# ===== 递归修改（危险！慎用） =====
chmod -R 755 project/        # 递归修改目录下所有文件/子目录
# ⚠️ -R 要小心：目录需要 x 才能进入，普通文件一般不需要 x

# 推荐用下面这个命令区分目录和文件：
find project/ -type d -exec chmod 755 {} \;    # 目录 755
find project/ -type f -exec chmod 644 {} \;    # 文件 644
```

#### 用户/组/其他人的字母代号

| 字母 | 含义 | 示例 |
|------|------|------|
| `u` | user（所有者） | `u+x` — 所有者加执行 |
| `g` | group（所属组） | `g-w` — 组去写权限 |
| `o` | others（其他人） | `o=` — 清空其他人权限 |
| `a` | all（全部三类） | `a+r` — 所有人加读权限 |

### 1.5 chown / chgrp — 修改文件归属

```bash
# chown = Change Owner，修改所有者（和所属组）
chown user1 file.txt                    # 所有者改为 user1
chown user1:group1 file.txt             # 所有者改为 user1，组改为 group1
chown -R www-data:www-data /var/www/    # 递归修改（Web 服务器常用）

# chgrp = Change Group，只修改所属组
chgrp devops file.txt                   # 所属组改为 devops

# 查看当前用户和组
whoami                                   # 我是谁
groups                                   # 我在哪些组
id                                       # 详细信息（uid/gid/所有组）
```

#### 🧪 测试场景：部署脚本权限排查

模拟一次典型的部署故障排查过程：

```bash
# 场景：开发说"部署后脚本执行不了"
# Step 1：看权限
ls -l /opt/app/start.sh
# 输出：-rw-r--r--  1 root root  120 Jul 11 10:00 start.sh
#               ↑ 没有 x！所有人只有 r--

# Step 2：看所有者
# 应用是 www-data 用户跑的，但文件属于 root
# Step 3：修复
sudo chown www-data:www-data /opt/app/start.sh    # 改归属
sudo chmod 755 /opt/app/start.sh                  # 加执行权限

# Step 4：验证
ls -l /opt/app/start.sh
# 输出：-rwxr-xr-x  1 www-data www-data  120 Jul 11 10:00 start.sh
#               ↑ 修复完成
```

> 💡 **测试工程师的权限排查三问**：① 文件有没有执行权限（x）？② 文件属于谁（用应用用户还是 root）？③ 目录有没有进入权限（目录也需要 x 才能 cd 进去）？

### 1.6 umask — 新建文件的默认权限

```bash
umask          # 查看当前掩码，通常是 0022 或 0002
# 简单理解：umask 022 意味着新建文件默认 644 (666-022)，新建目录默认 755 (777-022)
```

> 这部分了解即可，运维面试偶尔会问。测试工程师不需要改 umask。

---

## 第二部分：进程管理

> 🎯 测试工程师的核心场景：**"帮我看一下 XX 服务还在不在运行"** / **"有个进程卡死了，帮我杀掉"**

### 2.1 ps — 查看进程快照

```bash
# ===== 最常用：ps aux（BSD 风格，无横杠） =====
ps aux              # 显示所有用户的所有进程
# 输出列：
# USER   PID  %CPU %MEM    VSZ   RSS TTY   STAT START   TIME COMMAND
# root     1   0.0  0.1 168196 13344 ?     Ss   09:00   0:01 /sbin/init
# 关键列：PID(进程ID)、%CPU、%MEM、STAT(状态)、COMMAND(命令)

# ===== ps -ef（Unix 风格，带横杠） =====
ps -ef              # 另一种常用格式，显示 PPID（父进程ID）

# ===== 过滤特定进程 =====
ps aux | grep nginx          # 查找 nginx 进程
ps aux | grep java            # 查找 Java 应用
ps -u www-data                # 查看指定用户的进程

# ===== 只看 PID 和命令 =====
ps -eo pid,comm               # 自定义输出列
```

#### 进程状态（STAT 列）速查

| 状态 | 含义 | 测试要关心的 |
|------|------|-------------|
| `S` | Sleeping（睡眠中，等待事件） | ✅ 正常——大多数进程都是这个状态 |
| `R` | Running（正在运行或在队列中） | ✅ 正常 |
| `D` | 不可中断睡眠（等 I/O） | ⚠️ 短暂出现正常，长时间=磁盘可能有问题 |
| `Z` | Zombie（僵尸进程） | 🔴 **需关注**——进程已死但父进程没回收 |
| `T` | Stopped（被暂停） | ⚠️ 可能是被 `Ctrl+Z` 挂起了 |
| `<` | 高优先级 | ℹ️ 了解即可 |
| `N` | 低优先级 | ℹ️ 了解即可 |
| `+` | 前台进程组 | ℹ️ 了解即可 |

### 2.2 top — 实时进程监控

```bash
top                 # 启动实时监控（按 q 退出）

# 常用交互按键（在 top 界面内按）：
#   1         — 查看每个 CPU 核心的使用率
#   M         — 按内存使用排序（大写 M）
#   P         — 按 CPU 使用排序（大写 P）
#   k         — 杀死进程（输入 PID + 信号）
#   q         — 退出

# htop（更友好的替代品，需单独安装）
htop                # 彩色界面，鼠标可点击，比 top 直观
```

#### top 上半部分信息解读

```
top - 14:30:15 up 30 days,  2:15,  2 users,  load average: 0.15, 0.20, 0.18
#     ↑ 当前时间  ↑ 运行30天  ↑2个在线  ↑ 1分钟/5分钟/15分钟平均负载
#     load average < CPU 核心数 = 正常；持续 > CPU 核心数 = 系统繁忙

Tasks: 156 total,   1 running, 155 sleeping,   0 stopped,   0 zombie
#       ↑ 总进程   ↑ 运行中     ↑ 睡眠中      ↑ 暂停       ↑ 僵尸
#       zombie 数量 > 0 → 需要排查

%Cpu(s):  2.5 us,  1.0 sy,  0.0 ni, 96.5 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
#         ↑用户态  ↑内核态           ↑空闲    ↑等IO           ↑虚拟机偷取
#         us 高=应用忙  sy 高=系统调用多  wa 高=磁盘IO瓶颈  id ~0=CPU打满

MiB Mem :  15892.1 total,   3210.5 free,   8760.2 used,   3921.4 buff/cache
MiB Swap:   2048.0 total,   1800.5 free,    247.5 used.   6500.1 avail Mem
#         ↑总内存              ↑真实空闲              ↑交换分区使用
#         swap used 很高 = 内存不够用了，进程在"以空间换时间"
```

> 💡 **测试工程师看 top 的三步法**：① 看 load average 是否飙升；② 看 CPU 或 Memory 谁最高是哪个进程；③ 看有没有 zombie（僵尸进程）。

### 2.3 kill — 终止进程

```bash
# kill 本质是"发信号"，不是"杀死"——不同的信号有不同的含义
kill PID                # 默认发 -15 (SIGTERM)，礼貌地请进程退出
kill -9 PID             # 强制杀 (SIGKILL)，不给进程清理的机会
kill -l                 # 查看所有信号列表

# ===== 常用信号 =====
# -1  (SIGHUP)   — 重新加载配置文件（不重启进程）
# -2  (SIGINT)   — 等同于 Ctrl+C，中断前台进程
# -9  (SIGKILL)  — 强制终止（最后手段，进程无法捕获此信号）
# -15 (SIGTERM)  — 优雅终止（默认，给进程机会清理资源）
# -19 (SIGSTOP)  — 暂停进程
# -18 (SIGCONT)  — 恢复暂停的进程

# ===== 按名称杀进程 =====
pkill nginx                     # 按名字杀
pkill -f "python app.py"        # 匹配完整命令行
killall java                    # 杀所有同名进程（小心！）

# ===== 查找并杀死（组合技） =====
ps aux | grep "redis" | grep -v grep | awk '{print $2}' | xargs kill
#  ↑找redis  ↑排除grep自己   ↑提取PID            ↑逐个杀
#  ps aux | grep redis | awk '{print $2}' | xargs kill -9  # 强制版
```

> ⚠️ **信号使用原则**：先用 `-15`（优雅），不响应再用 `-9`（强制）。`kill -9` 就像拔电源——进程来不及保存数据、关闭网络连接。

### 2.4 前后台切换

```bash
# ===== 前台 → 后台 =====
sleep 300           # 前台运行，终端被占用 300 秒
# 按 Ctrl+Z          # 暂停并放到后台
bg                  # 让它在后台继续运行

# ===== 直接后台启动 =====
sleep 300 &         # 末尾加 & = 后台运行
# 输出：[1] 12345   ← [作业号] PID

# ===== 查看后台任务 =====
jobs                # 查看当前终端的后台任务
# [1]+ Running   sleep 300 &
# [2]- Stopped    python app.py   (被 Ctrl+Z 暂停的)

# ===== 后台 → 前台 =====
c                  # 把最近的后台任务切回前台
fg %1               # 把作业号 1 切回前台
fg 12345            # 把 PID 12345 切回前台
```

### 2.5 nohup — 终端关了也不停

```bash
# 问题：SSH 连接断开后，通过 & 启动的后台进程也会被杀
# 解决：nohup = No Hang Up，免疫 SIGHUP 信号

nohup python app.py &          # 后台运行，断开SSH也不停
nohup python app.py > app.log 2>&1 &  # 同时重定向输出到日志

# 输出默认写入 nohup.out，可以用 tail -f 查看
tail -f nohup.out
```

#### 🧪 测试场景：排查"应用挂了没"

```bash
# Step 1：应用在不在？
ps aux | grep "app.py" | grep -v grep
# 有输出 → 在；无输出 → 挂了或没启动

# Step 2：如果在但没响应 → 看是否变僵尸了
ps aux | grep "app.py"
# STAT 列如果是 Z → 僵尸进程，父进程可能有问题

# Step 3：看资源占用（是不是内存泄漏导致OOM）
top -p PID          # 只看指定进程
# 观察 %MEM 是否随时间持续增长

# Step 4：看进程在监听哪个端口（结合网络工具）
netstat -tlnp | grep PID
```

---

## 第三部分：网络工具

> 🎯 测试工程师的核心场景：**"这个接口通不通？"** / **"端口被谁占了？"** / **"远程服务在不在？"**

### 3.1 ping — 网络连通性

```bash
ping 8.8.8.8                        # 持续 ping（按 Ctrl+C 停止）
ping -c 4 192.168.1.1               # 只发 4 个包
ping -c 4 google.com                # 域名也可以（测试 DNS 是否正常）

# 输出解读：
# 64 bytes from 8.8.8.8: icmp_seq=1 ttl=117 time=12.3 ms
#                                    ↑TTL=跳数  ↑延迟
# 
# --- 8.8.8.8 ping statistics ---
# 4 packets transmitted, 4 received, 0% packet loss, time 3004ms
#                              ↑丢包率 = 0% ✅
# 丢包率 > 0% → 网络不稳定
# 丢包率 = 100% → 不通（目标不可达/防火墙拦截/对方禁 ping）
```

> ⚠️ **ping 不通 ≠ 服务不可用**：很多服务器禁用了 ICMP（ping 协议），但 HTTP/HTTPS 端口是通的。所以排查网络问题时，ping 只是第一步。

### 3.2 curl — HTTP 接口测试利器

这是测试工程师**每天都会用到**的命令。

```bash
# ===== GET 请求 =====
curl https://api.example.com/health          # 输出响应体
curl -I https://api.example.com              # 只看响应头（HEAD 请求）
curl -v https://api.example.com/health       # 详细模式（看请求/响应全过程）
curl -o /dev/null -s -w "%{http_code}" https://api.example.com
#   ↑不保存内容  ↑静默  ↑输出HTTP状态码：返回 200 200 或 503

# ===== POST 请求 =====
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"123456"}'
#   -X  指定方法（GET/POST/PUT/DELETE）
#   -H  加请求头
#   -d  请求体数据

# ===== 常用组合 =====
curl -X POST http://localhost:8080/api/user \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer token123" \
  -d '{"name":"张三","age":25}'

# ===== 测试场景：快速判断服务是否正常 =====
curl -s -o /dev/null -w "HTTP状态码: %{http_code}\n响应时间: %{time_total}s\n" \
  http://localhost:8080/health
# 输出：HTTP状态码: 200
#       响应时间: 0.023s

# ===== 下载文件 =====
curl -O https://example.com/file.tar.gz     # 保存为原文件名
curl -o myfile.tar.gz https://example.com/file.tar.gz  # 自定义文件名
```

#### curl 常用选项速查

| 选项 | 含义 | 测试场景 |
|------|------|----------|
| `-I` | 只获取响应头 | 快速确认服务是否在运行 |
| `-v` | 详细模式 | 调试 SSL/TLS 握手、看完整交互 |
| `-s` | 静默模式 | 脚本中使用 |
| `-o` | 输出到文件 | 下载文件 |
| `-w` | 自定义输出格式 | 提取 HTTP 状态码、响应时间 |
| `-X` | 指定 HTTP 方法 | GET/POST/PUT/DELETE |
| `-H` | 添加请求头 | Content-Type、Authorization |
| `-d` | POST 请求体 | 提交 JSON/表单数据 |
| `-k` | 跳过 SSL 证书验证 | 测试自签名证书的环境 |
| `-L` | 跟随重定向 | 301/302 自动跳转 |

### 3.3 netstat / ss — 查看端口占用

```bash
# netstat（传统工具，输出更详细）和 ss（现代工具，更快）

# ===== netstat 常用组合 =====
netstat -tlnp            # 只看 TCP 监听端口 + 不解析域名 + 显示进程
# -t TCP  -u UDP  -l 监听中的  -n 数字格式(不解析)  -p 显示进程
# Proto Local Address      State       PID/Program
# tcp    0.0.0.0:8080      LISTEN      12345/java
# tcp    127.0.0.1:3306    LISTEN      6789/mysqld
# ↑              ↑端口号    ↑LISTEN=正在监听

netstat -an | grep 8080   # 看某个端口的全部连接（含已建立的）
# ESTABLISHED = 连接已建立（有人正在用）
# TIME_WAIT   = 连接关闭后的等待期（大量出现说明短连接频繁）
# LISTEN       = 正在监听

# ===== ss（推荐，速度更快） =====
ss -tlnp                  # 等同于 netstat -tlnp，但速度更快
ss -s                     # 统计摘要

# ===== 找出谁占用了端口（高频操作）=====
lsof -i :8080             # 查看 8080 端口被哪个进程占用
# 或者
ss -tlnp | grep :8080
```

> 💡 **`127.0.0.1:3306` vs `0.0.0.0:8080`**：127.0.0.1 = 只能本机访问（安全），0.0.0.0 = 所有网络接口都可访问（对外暴露）。MySQL 默认只监听 127.0.0.1 就是为了安全。

#### 🧪 测试场景：排查"服务为什么挂了"

模拟一个完整的故障排查过程：

```bash
# ==== 场景：开发说 API 服务连不上 ====

# Step 1：主机通不通？
ping -c 2 192.168.1.100
# 通 → 网络层 OK，往下查端口

# Step 2：进程在不在？
ssh user@192.168.1.100 "ps aux | grep java | grep -v grep"
# 不在 → 确认服务挂了，通知运维重启
# 在   → 往下查端口

# Step 3：端口有没有在监听？
ssh user@192.168.1.100 "ss -tlnp | grep :8080"
# 无输出      → 进程在但端口没开，可能配置错误或启动失败
# LISTEN      → 端口开着，往下查应用层

# Step 4：直接请求接口试一下
curl -v http://192.168.1.100:8080/health
# 200 正常    → 可能是客户端网络/DNS 问题
# 503/超时    → 服务有问题或有防火墙拦截
# Connection refused → 端口没开（回到 Step 3）

# Step 5：看最近日志找原因
ssh user@192.168.1.100 "tail -100 /var/log/app/error.log | grep -B 2 -A 2 'ERROR\|FATAL\|Exception'"
```

> 📌 **排查顺序口诀**：**先 ping 通，再看进程，三查端口，四 curl 请求。** 不要跳过前面直接查后面的——每一步排除一层，才能准确定位问题。

---

## 📝 第八天课后练习

### 练习一：权限计算（5 题）

1. `rwxr-xr--` 对应的数字权限是多少？
   chmod 754 ...
2. `chmod 644` 后，**其他人**对这个文件有什么权限？
   可读
3. 一个脚本需要"所有者能读写执行、同组人能读执行、其他人无任何权限"，用数字表示法怎么写？
   750
4. 以下操作哪里有问题？`chmod 777 /etc/passwd`
   /etc目录下都是一些很重要的配置文件，passwd存放用户信息的文件，非常重要，除了对于所有者其他的人权限不应该给太高，否则具有很大的权限安全等风险
5. 部署了一个 Web 应用，目录结构如下，应该用什么命令修复权限？
   ```
   drw-r--r--  root root  /opt/webapp/    （目录，其他人进不去）
   -rw-r--r--  root root  /opt/webapp/index.html		
   
   
   chmod a+x /opt/webapp
   ```

### 练习二：进程管理（5 题）

1. 如何查看系统中所有包含 "nginx" 的进程？
   ps aux | grep "nginx" | grep -v grep
2. 有一个进程 PID=9527 卡死无响应，如何优雅地终止它？如果优雅终止无效呢？

   1. kill -15 9527
   2. ·`终止无效`： kill -9 9527
3. 启动一个后台任务 `python test_server.py &` 后，如何把它调回前台？

   1. fg
4. 如何让一个命令在终端关闭后仍然运行？（写出完整命令）
   nohup python app.py &
5. 以下 top 输出中有什么问题需要关注？
   ```
   load average: 8.50, 7.20, 6.80  （机器是 4 核）
   Tasks: 200 total, 1 running, 195 sleeping, 0 stopped, 4 zombie
   
   当前一分钟负载8.5核，5分钟7.2核，15分钟平均负载6.8核，CPU 严重打满，大量进程等待调度；近 15 分钟负载持续 6.8 以上，长期高负载。
   Tasks : 总共200个进程 1个正在运行 195个处于睡眠 0个暂停 4个僵尸进程 需关注4个僵尸进程已死但父进程没回收
   ```

### 练习三：网络工具（4 题）

1. 如何快速确认 `http://192.168.1.100:8080/api/health` 接口是否可访问，并只获取 HTTP 状态码？
   curl -I http://192.168.1.100:8080/api/health 
2. 启动 Java 应用时提示 "端口 8080 已被占用"，用什么命令找出是谁占的？
   ss -tulnp | grep "8080"
3. ping 某个服务器 100% 丢包，但 curl 能正常访问该服务器的 Web 页面，为什么？
   ping 和网页走不同协议，网络设备可以单独拦截 ICMP 而放行 TCP，因此出现 ping 100% 丢包但 Web 访问正常。
4. 写出排查"API 服务连不上"的完整命令序列（从网络层到应用层）。

```
# Step 1：主机通不通？
ping 192.168.200.1
# Step 2：进程在不在？
ssh user@192.168.200.1 不在找运维
在 -> 查进程端口 ps -aux | grep java | grep -v grep
# Step 3：端口有没有在监听？
ssh user@192.168.200.1 先测连通
ss -tlnp | grep "8080"
# Step 4：直接请求接口试一下
curl -v http://192.168.200.1:8080/health
200 能请求成功，可能是网络或者dns的问题
503 服务停机、过载、维护中找运维
# Connection refused → 端口没开（回到 Step 3）
# Step 5：看最近日志找原因
ssh user@192.168.200.1 
tail -n 100 /var/log/app/app.log | grep -C 2 "ERROR\|FATAL\|Exception"
```



### 练习四：综合排查（2 题）

1. 开发说"部署后脚本执行不了"，请写出完整的排查命令序列（权限 + 归属角度）。
   1. ls -al run.sh
   2. 查看文件是否具有x权限
   3. 不具有 chmod 755 run.sh
   4. 具有权限 ：查看开发用户所属组是否在组内，不在usermod -aG group1 user1
   
   ``` 
   1. ls -l run.sh 看所有者和权限
   2. 如果所有者不对 → `chown 应用用户:应用组 run.sh`（这是教材重点！）
   3. 如果权限缺 x → `chmod 755 run.sh`
   4. 验证：`ls -l run.sh` 确认修复成功
   ```
   
   ```
   # Step 1: 看权限+归属
   ls -la /opt/app/run.sh
   # Step 2: 改归属（如需要）
   sudo chown www-user:www-group /opt/app/run.sh
   # Step 3: 加执行权限
   sudo chmod 755 /opt/app/run.sh
   # Step 4: 也检查目录权限
   ls -ld /opt/app/
   # Step 5: 验证
   sudo -u www-user /opt/app/run.sh
   ```
   
   
   
2. 生产环境 `app.log` 中发现大量 "OutOfMemoryError"，请写出 3 条命令来了解当前系统状态。
   1. ps -aux | grep java
   2. top
   3. htop
   4. `free -h`（看内存使用情况，OOM 的核心指标）
   5.  `dmesg \| tail -50 \| grep -i oom`（看 OOM Killer 有没有杀进程）。

---

## 📋 第八天自检清单

完成学习后，确认以下知识点：

- [ ] 能脱口而出 755、644、700、600 分别对应什么权限字符串
  rwxr-xr-x rw-r--r-- rwx------ rw-------

- [ ] 知道 `chmod u+x` 和 `chmod 755` 的区别（符号 vs 数字）

  1. 给当前文件的执行权限添加给所有者
  2. 把当前文件的权限设置为rwxr-xr-x

- [ ] 知道 `chown` 和 `chmod` 分别改什么（归属 vs 权限）
  更改所属人和组 更改文件权限

- [ ] 能用 `ps aux | grep xxx` 查找进程

- [ ] 能读懂 top 的 load average 和 zombie 数
  平均负载 和 僵尸进程

- [ ] 知道 `kill` 和 `kill -9` 的区别（优雅 vs 强制）

- [ ] 知道 `&` 和 `nohup` 的区别（普通后台 vs 免疫 HUP）

- [ ] 能用 `curl -X POST -H -d` 发一个 JSON 请求

  curl -X POST https://api.example.com/login \
    -H "Content-Type: application/json" \
    -d '{"username":"admin","password":"123456"}'

- [ ] 能用 `ss -tlnp` 或 `lsof -i :端口` 查端口占用

- [ ] 能独立完成"ping → ps → ss → curl"四步排查链路

---

## 🔜 下一天预告

**第九天：Shell 脚本入门**

掌握了这么多命令后，第九天将学习如何把它们写成脚本：
- 变量、条件判断（`if`/`case`）、循环（`for`/`while`）
- 函数定义与调用
- 脚本参数（`$1`/`$2`/`$@`）
- 退出码与错误处理（`$?`、`set -e`）
- 实战：写一个服务健康检查脚本

---

> ✨ **第八天的核心心法**：权限、进程、网络是测试工程师登录服务器的 **"三把刀"**。权限解决"能不能做"，进程解决"还在不在做"，网络解决"通不通"。三个维度交叉验证，才能准确定位"服务为什么挂了"。

---

## 📚 命令速查表（Day 6~8 汇总）

### 权限相关
| 命令 | 用途 | 常用示例 |
|------|------|----------|
| `chmod 755 file` | 数字法改权限 | rwxr-xr-x |
| `chmod u+x file` | 符号法加执行 | 给所有者 +x |
| `chown user:group file` | 改所有者+组 | 部署后改归属 |
| `ls -l` | 查看权限 | 每个文件都先看这个 |

### 进程相关
| 命令 | 用途 | 常用示例 |
|------|------|----------|
| `ps aux` | 查看所有进程 | `ps aux \| grep nginx` |
| `top` | 实时监控 | 按 M(内存)/P(CPU)/q(退出) |
| `kill PID` | 优雅终止 | 默认 -15 |
| `kill -9 PID` | 强制终止 | 最后手段 |
| `nohup cmd &` | 后台不中断 | SSH 断开也不停 |

### 网络相关
| 命令 | 用途 | 常用示例 |
|------|------|----------|
| `ping -c 4 host` | 测连通性 | 第一步排查 |
| `curl -I url` | 看响应头 | 快速确认服务 |
| `curl -X POST -H -d` | 发请求 | 接口测试 |
| `ss -tlnp` | 查看监听端口 | 找端口占用 |
| `lsof -i :8080`= ss -tlnp \| grep 8080 | 谁占了这个端口 | 端口冲突排查 |
