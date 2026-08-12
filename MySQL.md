# 第一天：MySQL 基础入门（一）— 数据库概念与环境搭建

> 📅 学习日期：第一天 | 🎯 模块：MySQL 数据库 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：为什么测试工程师必须学数据库？

功能测试和 Linux 模块结束后，你可能会问："我测的是界面和接口，为什么要学数据库？"

答案很简单：**每一个软件系统的核心都是数据。** 作为测试工程师，你迟早会遇到这些场景：

| 场景 | 需要的数据库技能 |
|------|-----------------|
| 🔍 **验证测试结果** | 注册了一个用户，去数据库查一下到底写入成功了没 |
| 🧪 **构造测试数据** | 批量生成 1000 个测试账号，手工注册不现实 |
| 🗑️ **清理测试环境** | 每次跑完自动化用例，把脏数据清理干净 |
| 🐛 **定位 Bug 根因** | 前端显示"下单失败"，去数据库看是哪个字段的问题 |
| 📊 **数据对比验证** | 接口返回的订单金额和数据库里的是否一致 |

> 🎯 **一句话**：不会数据库的测试工程师，只能测"表面现象"——你知道前端报错了，但不知道是哪张表、哪个字段、哪条数据出了问题。

---

## 一、数据库是什么？

### 1.1 一句话定义

**数据库（Database）** 是按一定结构组织、存储和管理数据的"仓库"。

通俗来说：数据库就像是**结构化的超级 Excel**——Excel 能存数据、排序、筛选、计算，数据库也能，但数据库能存**百万级数据**、支持**多人同时操作**、有**安全权限控制**、还能**保证数据不丢**。

### 1.2 为什么不用 Excel？

| 对比维度 | Excel | MySQL 数据库 |
|----------|-------|-------------|
| 📦 **数据量** | 几万行就开始卡 | 数千万行轻松应对 |
| 👥 **并发访问** | 同一时间只能一个人编辑 | 成百上千用户同时读写 |
| 🔒 **安全性** | 设个密码就能打开 | 精细到每张表的增删改查权限 |
| 🔗 **数据关联** | VLOOKUP 跨表查，性能差 | JOIN 多表联查，毫秒级 |
| 🔄 **事务保证** | 改错了 Ctrl+Z | 事务机制保证数据一致性 |
| 🏗️ **程序调用** | 需要人工打开文件 | 代码通过 SQL 直接操作 |

### 1.3 关系型数据库核心概念

> ⚠️ **面试高频考点，建议熟记**

MySQL 是**关系型数据库管理系统（RDBMS）**，核心概念如下：

| 概念 | 通俗类比 | 说明 |
|------|----------|------|
| **数据库（Database）** | 一个 Excel 文件 | 包含多张表的容器 |
| **表（Table）** | Excel 中的一个 Sheet | 存储同一类数据的二维表格 |
| **列/字段（Column/Field）** | Sheet 中的一列 | 描述数据的一个属性（如"用户名""密码"） |
| **行/记录（Row/Record）** | Sheet 中的一行 | 一条完整的数据（如一个用户的所有信息） |
| **主键（Primary Key）** | 身份证号 | 唯一标识一行数据的字段，不能重复、不能为空 |
| **外键（Foreign Key）** | 关联引用 | 一张表中的字段引用另一张表的主键 |
| **索引（Index）** | 书的目录 | 加速数据查找的数据结构 |
| **SQL** | 和数据库对话的语言 | 结构化查询语言，所有操作都通过 SQL 完成 |

### 1.4 一条数据的一生

```
用户在前端注册账号
        │
        ▼
后端代码收到请求（如 POST /api/register）
        │
        ▼
后端执行 SQL：INSERT INTO users (username, password) VALUES ('zane', 'xxx')
        │
        ▼
MySQL 将这条数据写入 users 表
        │
        ▼
返回"注册成功"给前端
```

作为测试工程师，你需要验证这条链路中的**每一步**——从"前端显示注册成功"到"数据库里确实多了一条数据"。

---

## 二、MySQL 环境搭建

### 2.1 MySQL 是什么？

**MySQL** 是目前最流行的开源关系型数据库。它免费、稳定、资料丰富，是学习数据库的最佳选择。

> 📌 面试中"关系型数据库"默认指 MySQL，除非特别说明。

### 2.2 在 Windows 上安装 MySQL

#### 方式一：使用 XAMPP（推荐，最简单）

```
1. 下载 XAMPP：https://www.apachefriends.org/
2. 安装时勾选 MySQL（XAMPP 默认包含 MariaDB，它是 MySQL 的分支，语法 100% 兼容）
3. 打开 XAMPP Control Panel，点击 MySQL 旁边的 "Start"
4. 点击 MySQL 旁边的 "Shell" 进入 MySQL 命令行
```

> 💡 XAMPP 自带 phpMyAdmin（网页版数据库管理工具），在浏览器访问 `http://localhost/phpmyadmin` 即可。

#### 方式二：直接安装 MySQL（推荐用于生产学习）

```
1. 下载 MySQL Installer：https://dev.mysql.com/downloads/installer/
2. 选择 "MySQL Server" + "MySQL Workbench"（图形化管理工具）
3. 安装时设置 root 密码（务必记住！）
4. 安装完成后，MySQL Workbench 可以图形化操作数据库
```

#### 方式三：使用 Docker（如果你已安装 Docker）

```bash
docker run --name mysql-test -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 -d mysql:8.0
```

### 2.3 如何确认 MySQL 安装成功？

打开终端（CMD / PowerShell / Git Bash），输入：

```bash
mysql -u root -p
```

输入密码后，如果看到以下界面，说明安装成功：

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.35 MySQL Community Server - GPL

mysql>
```

> 📌 `mysql>` 是 MySQL 的命令行提示符，之后所有 SQL 命令都在这里输入。

### 2.4 MySQL 命令行基础操作

```sql
-- 查看所有数据库
SHOW DATABASES;

-- 创建数据库（测试学习用）
CREATE DATABASE test_study;

-- 使用某个数据库
USE test_study;

-- 查看当前数据库中的所有表
SHOW TABLES;

-- 退出
EXIT;
```

> ⚠️ **重要提醒**：SQL 命令必须以 `;`（分号）结尾。如果忘了分号，会看到 `->` 提示符，表示 MySQL 在等你继续输入。此时输入 `;` 然后按回车即可。

---

## 三、数据类型 — 建表前必须选好"容器"

### 3.1 为什么数据类型很重要？

建表时，每个字段必须指定**数据类型**。这和编程语言中声明变量类型是同一个道理——数据库需要知道这个字段存的是数字还是字符串，以便合理分配存储空间和校验数据。

> 🎯 选错数据类型 = 给测试埋坑。比如用 `INT` 存手机号 → 0 开头的号码（如 021-12345678）前面的 0 会被丢掉。

### 3.2 MySQL 常用数据类型

#### 数值类型

| 类型 | 范围 | 测试常用来存什么 |
|------|------|-----------------|
| **INT** | -21 亿 ~ 21 亿 | 用户 ID、数量、年龄 |
| **BIGINT** | 超大整数 | 订单号（可能超 21 亿）、时间戳（毫秒） |
| **DECIMAL(M,D)** | 精确小数 | **金额**（`DECIMAL(10,2)` = 最多 10 位，其中 2 位小数） |
| **FLOAT/DOUBLE** | 近似小数 | 科学计算，**不用来存金额！** |

> 🔴 **重要**：存金额必须用 `DECIMAL`，不能用 `FLOAT`！因为 `FLOAT` 是近似值，`0.1 + 0.2` 可能不等于 `0.3`。

#### 字符串类型

| 类型 | 特点 | 测试常用来存什么 |
|------|------|-----------------|
| **CHAR(N)** | 定长，固定占 N 个字符 | 手机号（固定 11 位）、身份证号（固定 18 位） |
| **VARCHAR(N)** | 变长，最多 N 个字符 | 用户名、密码、邮箱、地址 |
| **TEXT** | 长文本，最大 64KB | 文章内容、日志详情、JSON 数据 |
| **LONGTEXT** | 超长文本，最大 4GB | 超大日志文件 |

> 📌 **CHAR vs VARCHAR**：CHAR 定长适合"长度固定的数据"（手机号、身份证），VARCHAR 变长适合"长度不固定的数据"（用户名、地址）。VARCHAR 更省空间，但 CHAR 查询更快。

#### 日期时间类型

| 类型 | 格式 | 测试常用来存什么 |
|------|------|-----------------|
| **DATE** | `YYYY-MM-DD` | 生日、注册日期 |
| **DATETIME** | `YYYY-MM-DD HH:MM:SS` | 创建时间、修改时间、支付时间 |
| **TIMESTAMP** | 同 DATETIME，但自动转时区 | 记录时间戳（2038 年前可用） |

> 📌 测试中最常用的是 **DATETIME**——每次创建/修改数据时记录时间，用于验证"这个订单是什么时候下的"。

#### 其他常用类型

| 类型 | 说明 |
|------|------|
| **BOOLEAN** | 真/假（实际存储为 TINYINT，0 或 1） |
| **ENUM('A','B','C')** | 枚举，只能从预设值中选一个（如性别、状态） |

---

## 四、SQL DDL — 创造你的第一张表

### 4.1 什么是 DDL？

SQL 按功能分为三大类：

| 类别 | 全称 | 作用 | 关键词 |
|------|------|------|--------|
| **DDL** | Data Definition Language | 定义数据库结构（建库、建表、改表、删表） | `CREATE` `ALTER` `DROP` |
| **DML** | Data Manipulation Language | 操作数据（增、删、改、查） | `INSERT` `UPDATE` `DELETE` `SELECT` |
| **DCL** | Data Control Language | 控制权限（授权、回收） | `GRANT` `REVOKE` |

> 🎯 今天学 DDL（建表），之后学 DML（操作数据）。

### 4.2 CREATE TABLE — 建表

#### 基本语法

```sql
CREATE TABLE 表名 (
    字段名1 数据类型 [约束],
    字段名2 数据类型 [约束],
    ...
    [表级约束]
);
```

#### 实战：创建第一张用户表

```sql
-- 创建一个用户表
CREATE TABLE users (
    id          INT PRIMARY KEY AUTO_INCREMENT,   -- 用户ID，主键，自增
    username    VARCHAR(50)  NOT NULL UNIQUE,     -- 用户名，最长50字符，不能空，不能重复
    password    VARCHAR(255) NOT NULL,            -- 密码（存加密后的密文，所以长一些）
    email       VARCHAR(100),                     -- 邮箱，可空（允许不填）
    phone       CHAR(11),                         -- 手机号，固定11位
    age         INT,                              -- 年龄
    balance     DECIMAL(10,2) DEFAULT 0.00,       -- 余额，默认0.00
    created_at  DATETIME DEFAULT CURRENT_TIMESTAMP, -- 创建时间，默认当前时间
    status      TINYINT DEFAULT 1                 -- 状态：1=正常，0=禁用
);
```

#### 逐行解读

| 字段 | 类型选择理由 | 约束说明 |
|------|-------------|----------|
| `id` | INT 足够，自增主键 | `PRIMARY KEY` = 主键（唯一+非空），`AUTO_INCREMENT` = 自动增长 |
| `username` | VARCHAR(50) 用户名长度不定 | `NOT NULL` = 不能为空，`UNIQUE` = 唯一（不能有两个同名用户） |
| `password` | VARCHAR(255) 加密后很长 | `NOT NULL` = 不能为空 |
| `email` | VARCHAR(100) 邮箱长度不定 | 允许为空（有些用户不填邮箱） |
| `phone` | CHAR(11) 手机号固定 11 位 | 允许为空 |
| `age` | INT 整数 | 允许为空 |
| `balance` | DECIMAL(10,2) 金额用精确小数 | `DEFAULT 0.00` = 不填则默认为 0.00 |
| `created_at` | DATETIME 日期时间 | `DEFAULT CURRENT_TIMESTAMP` = 不填则用当前时间 |
| `status` | TINYINT 存 0/1 | `DEFAULT 1` = 默认正常状态 |

### 4.3 约束（Constraint）— 数据的"安检门"

> ⚠️ **面试高频考点**

| 约束 | 作用 | 示例 |
|------|------|------|
| **PRIMARY KEY** | 主键：唯一标识一行，不能重复、不能为空 | `id INT PRIMARY KEY` |
| **FOREIGN KEY** | 外键：引用另一张表的主键 | `user_id INT REFERENCES users(id)` |
| **NOT NULL** | 非空：该字段不能为空 | `username VARCHAR(50) NOT NULL` |
| **UNIQUE** | 唯一：该字段值不能重复 | `email VARCHAR(100) UNIQUE` |
| **DEFAULT** | 默认值：不填时的默认值 | `status TINYINT DEFAULT 1` |
| **CHECK** | 检查：值必须满足条件（MySQL 8.0+） | `age INT CHECK (age >= 0 AND age <= 150)` |
| **AUTO_INCREMENT** | 自增：自动生成递增的数字 | `id INT AUTO_INCREMENT` |

### 4.4 从测试视角看约束

> 🎯 作为测试工程师，你需要为**每个约束**设计测试场景：

| 约束 | 正向测试（应该通过） | 反向测试（应该拒绝） |
|------|-------------------|---------------------|
| `NOT NULL` | 正常插入有值的数据 | 插入 NULL / 不传该字段 → 应报错 |
| `UNIQUE` | 插入不重复的值 | 插入重复值 → 应报错 |
| `PRIMARY KEY` | 插入唯一 ID | 插入重复 ID / NULL ID → 应报错 |
| `DEFAULT` | 不传值，验证是否用了默认值 | — |
| `CHECK` | 插入符合范围的值 | 插入超出范围的值 → 应报错 |

### 4.5 其他 DDL 操作

```sql
-- 查看表结构
DESC users;

-- 查看建表语句（含完整定义）
SHOW CREATE TABLE users;

-- 修改表：添加字段
ALTER TABLE users ADD COLUMN nickname VARCHAR(50);

-- 修改表：修改字段类型
ALTER TABLE users MODIFY COLUMN age TINYINT UNSIGNED;

-- 修改表：删除字段
ALTER TABLE users DROP COLUMN nickname;

-- 删除表（慎用！）
DROP TABLE users;

-- 重命名表
RENAME TABLE users TO tb_users;
```

> 🔴 `DROP TABLE` 会**永久删除**表结构和所有数据，不可恢复！生产环境操作前三思。

---

## 📝 今日练习

### 练习一：概念理解

1. 用自己的话解释：数据库、表、字段、记录分别是什么？用一个生活中的例子类比。
2. 主键和外键的区别是什么？为什么需要外键？
3. 为什么存金额不能用 `FLOAT`，而要用 `DECIMAL`？

### 练习二：数据类型选择

为以下场景选择合适的数据类型：

| 场景 | 你的答案 |
|------|----------|
| 用户的身份证号（18 位） | |
| 商品的名称（最长 200 字） | |
| 订单的总金额 | |
| 用户的注册时间 | |
| 用户的性别（男/女/未知） | |
| 文章的正文字段 | |
| 点赞数量 | |
| 用户的手机号（11 位） | |

### 练习三：设计第一张表

用 SQL DDL 创建一张 `products`（商品）表，要求包含以下字段：

- 商品 ID（主键、自增）
- 商品名称（不能为空、最长 100 字符）
- 价格（不能为空、精确到分）
- 库存数量（不能为空、默认为 0、不能为负数）
- 描述（允许为空、长文本）
- 创建时间（默认为当前时间）
- 状态（1=上架、0=下架，默认为 1）

### 练习四：测试视角分析

针对你创建的 `products` 表，分析每个约束，分别写出 1 条正向测试数据和 1 条反向测试数据。

| 约束 | 正向数据（应通过） | 反向数据（应拒绝） |
|------|-------------------|---------------------|
| PRIMARY KEY | | |
| NOT NULL（商品名称） | | |
| NOT NULL（价格） | | |
| DEFAULT（库存数量） | | |
| CHECK（库存 ≥ 0） | | |

---

## 📋 自检清单

学完今天的内容后，请用自己的话回答以下问题：

- [ ] 什么是关系型数据库？MySQL 属于哪一类？
- [ ] 数据库、表、字段、记录、主键、外键、索引分别是什么？
- [ ] MySQL 环境搭建完成了吗？能成功登录 `mysql>` 命令行吗？
- [ ] SQL 的 DDL / DML / DCL 分别是什么？各有哪些关键词？
- [ ] 为什么存金额用 `DECIMAL` 而不是 `FLOAT`？
- [ ] `CHAR` 和 `VARCHAR` 的区别是什么？各适合存什么？
- [ ] `DATETIME` 和 `TIMESTAMP` 的区别是什么？
- [ ] 能默写 `CREATE TABLE` 的基本语法吗？
- [ ] 6 种常见约束（PRIMARY KEY / FOREIGN KEY / NOT NULL / UNIQUE / DEFAULT / CHECK）分别是什么作用？
- [ ] `DROP TABLE` 和 `DELETE FROM` 的区别是什么（提示：表结构 vs 数据）？

---

> 🎯 **今日小结**：第一天我们建立了数据库的基本概念，知道了 MySQL 是什么、数据类型怎么选、怎么建表、以及约束的作用。这些都是数据库学习的"地基"——后面的增删改查（DML）、多表查询（JOIN）、索引优化等，全部建立在今天的知识之上。
>
> **第二天预告**：SQL DML — 增删改查（INSERT / UPDATE / DELETE / SELECT），开始真正操作数据！

---

# 第二天：SQL DML — 增删改查入门（INSERT / SELECT / UPDATE / DELETE）

> 📅 学习日期：第二天 | 🎯 模块：MySQL 数据库 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：DML 是什么？

第一天我们学了 DDL（建库建表），它定义了数据的"容器"。但空表没有意义，真正让数据库有价值的是**里面的数据**。

**DML（Data Manipulation Language，数据操作语言）** 就是用来操作数据本身的——往表里加数据、查数据、改数据、删数据。

> 🎯 **一句话**：DDL 是盖房子（建表），DML 是往房子里搬家具（操作数据）。

### 测试工程师的 DML 日常

| 测试场景 | 使用的 DML |
|----------|-----------|
| 注册了一个账号，去数据库确认写入成功 | `SELECT` 查询 |
| 批量造 1000 个测试用户 | `INSERT` 插入 |
| 修改某个用户的状态为"禁用" | `UPDATE` 更新 |
| 清理自动化用例产生的脏数据 | `DELETE` 删除 |

---

## 一、INSERT — 插入数据

### 1.1 单行插入

```sql
-- 基本语法
INSERT INTO 表名 (字段1, 字段2, ...) VALUES (值1, 值2, ...);

-- 实战：往 users 表插入一条用户数据
INSERT INTO users (username, password, email, phone, age, balance)
VALUES ('zane', 'hashed_password_123', 'zane@test.com', '13800138000', 25, 100.00);
```

> 📌 **字段和值一一对应**：字段列表和值列表的顺序、数量、类型必须匹配。

### 1.2 省略字段名（不推荐）

```sql
-- 如果不写字段名，必须按表定义顺序提供所有字段的值
INSERT INTO users VALUES (NULL, 'zane2', 'xxx', 'zane2@test.com', '13800138001', 28, 0.00, NOW(), 1);
```

> ⚠️ 这种写法**依赖字段顺序**，表结构一改就会出错。测试中**强烈建议写明字段名**。

### 1.3 多行批量插入

```sql
-- 一次插入多条数据（用逗号分隔）
INSERT INTO users (username, password, email, phone, age, balance)
VALUES
    ('user1', 'pass1', 'user1@test.com', '13800000001', 20, 50.00),
    ('user2', 'pass2', 'user2@test.com', '13800000002', 22, 80.00),
    ('user3', 'pass3', 'user3@test.com', '13800000003', 25, 100.00);
```

> 🎯 **测试场景**：批量造测试数据时，一次 `INSERT` 比多次单行插入快 10 倍以上。

### 1.4 插入部分字段

```sql
-- 只插入 username 和 password，其他字段使用默认值
INSERT INTO users (username, password) VALUES ('new_user', 'new_pass');
-- balance 会用 DEFAULT 0.00，created_at 会用 DEFAULT CURRENT_TIMESTAMP，status 会用 DEFAULT 1
```

### 1.5 INSERT INTO ... SELECT（高级用法）

```sql
-- 从一张表复制数据到另一张表
INSERT INTO users_backup (username, password, email)
SELECT username, password, email FROM users WHERE status = 1;
```

> 💡 这在测试环境中非常有用：从生产库抽取部分数据到测试库。

### 1.6 从测试视角看 INSERT

| 测试场景 | SQL 示例 |
|----------|----------|
| 验证自增主键 | `INSERT` 后 `SELECT LAST_INSERT_ID();` 查看生成的 ID |
| 验证默认值 | 只插必填字段，用 `SELECT` 检查默认字段是否生效 |
| 验证唯一约束 | 插入重复 `username`，预期报错 `Duplicate entry` |
| 验证非空约束 | 省略 `NOT NULL` 字段，预期报错 |
| 验证字段长度 | 插入超长 `username`（如 60 字符，但定义是 50），预期截断或报错 |

---

## 二、SELECT — 查询数据（最常用的 SQL）

> 🎯 SELECT 是使用频率最高的 SQL 语句。对测试工程师来说，**90% 的数据库操作都是查数据**。

### 2.1 基本查询

```sql
-- 查所有字段（⚠️ 生产环境慎用，数据量大时很慢）
SELECT * FROM users;

-- 查指定字段（推荐：只取需要的列）
SELECT id, username, email, balance FROM users;

-- 去重查询
SELECT DISTINCT status FROM users;
```

### 2.2 WHERE — 条件过滤

```sql
-- 等于
SELECT * FROM users WHERE username = 'zane';

-- 不等于（两种写法）
SELECT * FROM users WHERE status != 0;
SELECT * FROM users WHERE status <> 0;

-- 大于 / 小于 / 大于等于 / 小于等于
SELECT * FROM users WHERE age > 18;
SELECT * FROM users WHERE balance >= 100;
SELECT * FROM users WHERE created_at >= '2026-01-01';

-- BETWEEN（闭区间，包含两端）
SELECT * FROM users WHERE age BETWEEN 18 AND 30;

-- IN（多值匹配）
SELECT * FROM users WHERE status IN (0, 1, 2);

-- LIKE（模糊搜索）
SELECT * FROM users WHERE username LIKE 'zane%';    -- 以 zane 开头
SELECT * FROM users WHERE email LIKE '%@test.com';   -- 以 @test.com 结尾
SELECT * FROM users WHERE username LIKE '%user%';    -- 包含 user

-- IS NULL / IS NOT NULL（⚠️ 不能用 = NULL）
SELECT * FROM users WHERE phone IS NULL;
SELECT * FROM users WHERE email IS NOT NULL;

-- AND / OR / NOT（组合条件）
SELECT * FROM users WHERE age > 18 AND status = 1;
SELECT * FROM users WHERE balance < 0 OR balance > 10000;
SELECT * FROM users WHERE NOT status = 0;
```

> 🔴 **特别注意**：`NULL` 不能用 `=` 或 `!=` 判断！`WHERE phone = NULL` 永远返回空。正确写法：`IS NULL` / `IS NOT NULL`。

### 2.3 WHERE 的优先级陷阱

```sql
-- ❌ 错误：OR 优先级低于 AND，这条实际是：age > 18 OR (status = 1 AND balance > 100)
SELECT * FROM users WHERE age > 18 AND status = 1 OR balance > 100;

-- ✅ 正确：用括号明确优先级
SELECT * FROM users WHERE age > 18 AND (status = 1 OR balance > 100);
```

> 📌 **黄金法则**：WHERE 中有 `OR` 时，**永远用括号**把条件分组，不要依赖默认优先级。

### 2.4 ORDER BY — 排序

```sql
-- 升序（默认 ASC，可省略）
SELECT * FROM users ORDER BY created_at ASC;
SELECT * FROM users ORDER BY created_at;         -- 等价

-- 降序
SELECT * FROM users ORDER BY balance DESC;

-- 多字段排序：先按 status 升序，同 status 的按 created_at 降序
SELECT * FROM users ORDER BY status ASC, created_at DESC;
```

### 2.5 LIMIT — 限制返回行数

```sql
-- 只取前 10 行
SELECT * FROM users LIMIT 10;

-- 跳过前 5 行，取 10 行（分页查询）
SELECT * FROM users LIMIT 5, 10;     -- 第 6~15 行
SELECT * FROM users LIMIT 10 OFFSET 5; -- 等价写法（更推荐，可读性好）
```

> 🎯 **测试场景**：`LIMIT` 在大表中测试时非常关键——你只想验证几条数据，不加 `LIMIT` 可能扫全表。

### 2.6 SELECT 子句执行顺序

> ⚠️ **面试高频考点**

```
FROM → WHERE → SELECT → ORDER BY → LIMIT
```

理解这个顺序很重要！比如：

```sql
-- 为什么 WHERE 中不能用别名？
SELECT username AS name, age FROM users WHERE name = 'zane';  -- ❌ 报错！WHERE 在 SELECT 之前执行
SELECT username AS name, age FROM users WHERE username = 'zane'; -- ✅ 正确

-- 为什么 ORDER BY 中可以用别名？
SELECT username AS name, age FROM users ORDER BY name;  -- ✅ 正确！ORDER BY 在 SELECT 之后执行
```

### 2.7 从测试视角看 SELECT

```sql
-- 验证注册成功：查最新插入的用户
SELECT * FROM users ORDER BY created_at DESC LIMIT 1;

-- 验证数据完整性：注册后各字段是否正确
SELECT username, email, phone, status, balance, created_at
FROM users WHERE username = 'zane';

-- 验证状态流转：查所有被禁用的用户
SELECT COUNT(*) FROM users WHERE status = 0;

-- 验证边界：查余额异常的用户（负数或超大值）
SELECT * FROM users WHERE balance < 0 OR balance > 9999999;

-- 验证时间字段：查创建时间在未来的"穿越数据"
SELECT * FROM users WHERE created_at > NOW();
```

---

## 三、UPDATE — 修改数据

### 3.1 基本语法

```sql
UPDATE 表名 SET 字段1 = 值1, 字段2 = 值2 WHERE 条件;
```

### 3.2 实战示例

```sql
-- 修改单个字段
UPDATE users SET balance = 200.00 WHERE username = 'zane';

-- 修改多个字段
UPDATE users SET email = 'new_email@test.com', age = 26 WHERE id = 1;

-- 基于条件的批量更新
UPDATE users SET status = 0 WHERE created_at < '2025-01-01' AND status = 1;

-- 基于表达式的更新
UPDATE users SET balance = balance + 50 WHERE id = 1;  -- 余额加 50
UPDATE users SET age = age + 1 WHERE birthday = CURDATE(); -- 生日当天年龄 +1
```

### 3.3 🔴 WHERE 是最危险的遗漏

```sql
-- ❌ 致命错误：没有 WHERE 条件！
UPDATE users SET status = 0;  -- 会把所有用户都禁用！全表更新！
```

> 🎯 **安全习惯**：写 UPDATE 时，**先写 WHERE，再写 SET**：
> ```sql
> UPDATE users WHERE id = 1;  -- 先写这行
> UPDATE users SET status = 0 WHERE id = 1;  -- 再补全 SET
> ```

### 3.4 测试场景

```sql
-- 模拟用户修改密码
UPDATE users SET password = 'new_hashed_pass' WHERE username = 'zane';

-- 模拟充值
UPDATE users SET balance = balance + 100 WHERE id = 1;

-- 模拟账号封禁
UPDATE users SET status = 0 WHERE username = 'bad_user';

-- 验证更新成功
SELECT * FROM users WHERE username = 'zane';  -- 改完必须查！
```

> 📌 **测试铁律**：`UPDATE` 后必须 `SELECT` 验证。改了什么查什么。

---

## 四、DELETE — 删除数据

### 4.1 基本语法

```sql
DELETE FROM 表名 WHERE 条件;
```

### 4.2 实战示例

```sql
-- 删除指定用户
DELETE FROM users WHERE username = 'test_user';

-- 条件删除（删除 30 天前的禁用用户）
DELETE FROM users WHERE status = 0 AND created_at < DATE_SUB(NOW(), INTERVAL 30 DAY);

-- 删除所有数据（保留表结构）
DELETE FROM users;
```

### 4.3 DELETE vs TRUNCATE vs DROP

> ⚠️ **面试高频考点**

| 命令 | 删除内容 | 表结构 | 能否回滚 | 自增计数器 | 速度 |
|------|---------|--------|---------|-----------|------|
| `DELETE FROM 表` | 数据 | ✅ 保留 | ✅ 可回滚（事务中） | 不重置 | 慢（逐行删） |
| `TRUNCATE TABLE 表` | 数据 | ✅ 保留 | ❌ 不可回滚 | **重置为 1** | 快（整表删） |
| `DROP TABLE 表` | **数据 + 表结构** | ❌ 全删 | ❌ 不可回滚 | — | 最快 |

```sql
-- DELETE：可带 WHERE，可回滚
DELETE FROM users WHERE id > 100;

-- TRUNCATE：清空整张表，重置自增
TRUNCATE TABLE users;

-- DROP：整表删除，什么都没了
DROP TABLE users;
```

> 🎯 **测试环境常用**：`TRUNCATE TABLE` 用于每次测试前清空数据并重置自增 ID，保证测试数据的一致性。

### 4.4 🔴 和 UPDATE 一样，注意 WHERE

```sql
-- ❌ 致命错误：没有 WHERE 条件！
DELETE FROM users;  -- 全部数据没了！

-- ✅ 正确做法：先用 SELECT 确认要删的行
SELECT * FROM users WHERE status = 0 AND created_at < '2025-01-01';
-- 确认无误后，把 SELECT * 改成 DELETE
DELETE FROM users WHERE status = 0 AND created_at < '2025-01-01';
```

> 🎯 **安全铁律**：删除前先用 `SELECT` 看一遍要删的数据，确认行数对不对。测试环境养成这个习惯，生产环境才不翻车。

---

## 📝 今日练习

> 💡 以下练习假设你已创建了第一天的 `users` 表。如果没有，请先用以下 SQL 准备环境：
> ```sql
> CREATE TABLE users (
>     id INT PRIMARY KEY AUTO_INCREMENT,
>     username VARCHAR(50) NOT NULL UNIQUE,
>     password VARCHAR(255) NOT NULL,
>     email VARCHAR(100),
>     phone CHAR(11),
>     age INT,
>     balance DECIMAL(10,2) DEFAULT 0.00,
>     created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
>     status TINYINT DEFAULT 1
> );
> ```

### 练习一：INSERT 数据准备

1. 用一条 SQL 批量插入 3 个用户（用户名、密码、邮箱自拟）
2. 只插入 `username` 和 `password`，验证其他字段的默认值是否生效
3. 尝试插入一个 `username` 为 NULL 的用户，观察报错信息，理解为什么

### 练习二：SELECT 查询练习

针对你插入的数据，写出以下 SQL：

| 需求 | 你的答案 |
|------|----------|
| 查询所有状态为 1 的用户 | |
| 查询余额大于 50 的用户 | |
| 查询用户名包含 'test' 的用户 | |
| 按余额从高到低排序，只取前 5 条 | |
| 查询手机号为 NULL 的用户（即没填手机号） | |
| 查询今天创建的所有用户 | |
| 统计用户总数 | |

### 练习三：UPDATE 练习

1. 把用户 `user1` 的余额增加 50 元
2. 把所有余额为 0 的用户状态改为 0（禁用）
3. 改完后用 SELECT 验证修改是否成功

### 练习四：DELETE + 安全习惯

1. 先用 SELECT 查出余额小于 10 且状态为 0 的用户
2. 确认无误后，用 DELETE 删除这些用户
3. 删除后再次 SELECT 验证是否已删除干净

### 练习五：测试思维综合题

你正在测试一个"用户充值"功能。充值接口调用成功后，你需要去数据库验证。请列出你会在数据库中查什么、用什么 SQL。提示：至少包含以下验证点：

- 余额是否正确增加
- 是否有记录更新时间
- 充值前后余额差是否等于充值金额

---

## 📋 自检清单

- [ ] INSERT 的三种写法（全字段、指定字段、批量插入）都会了吗？
- [ ] INSERT 时违反 NOT NULL / UNIQUE 约束会怎样？
- [ ] `SELECT *` 和 `SELECT 字段列表` 各自的适用场景是什么？
- [ ] WHERE 中 `=` 和 `IS NULL` 的区别是什么？
- [ ] WHERE 中使用 OR 时为什么要加括号？
- [ ] `LIKE '%keyword%'` 的三个通配符位置（前/中/后）各是什么匹配模式？
- [ ] ORDER BY 的 ASC 和 DESC 各代表什么？
- [ ] LIMIT 的两个参数分别表示什么？`LIMIT 10, 5` 返回的是哪些行？
- [ ] SELECT 子句执行顺序（FROM → WHERE → SELECT → ORDER BY → LIMIT）记住了吗？
- [ ] UPDATE 和 DELETE 之前应该先做什么？安全习惯养成了吗？
- [ ] DELETE / TRUNCATE / DROP 三者的区别能脱口而出吗？
- [ ] UPDATE 后为什么要立即 SELECT 验证？

---

> 🎯 **今日小结**：第二天我们掌握了 DML 的四大操作——INSERT（增）、SELECT（查）、UPDATE（改）、DELETE（删）。其中 **SELECT 是重中之重**，测试工程师 90% 的数据库操作都在查数据。养成良好的 WHERE 习惯（UPDATE/DELETE 前先 SELECT 确认）是今天最重要的收获。
>
> **第三天预告**：SQL 进阶 — 聚合函数（COUNT/SUM/AVG/MAX/MIN）与 GROUP BY / HAVING，开始用 SQL 做数据分析！

---

# 第三天：聚合函数与分组查询 — 用 SQL 做数据分析

> 📅 学习日期：第三天 | 🎯 模块：MySQL 数据库 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：从"查数据"到"分析数据"

前两天我们学会了建表和基本增删改查。但实际测试中，你更多会遇到这样的需求：

- "统计今天注册了多少用户"
- "每个商品类别的平均价格是多少"
- "消费最高的前 10 个用户是谁"
- "支付失败率超过 5% 的商户有哪些"

这些都是**数据分析型查询**——不是简单地把数据取出来，而是对数据进行**汇总、分组、统计**。

> 🎯 **一句话**：基础 SELECT 是"找到那几条数据"，聚合查询是"从数据中提炼出结论"。

---

## 一、聚合函数 — SQL 的计算器

聚合函数对一组行进行计算，返回**单个值**。

### 1.1 五大常用聚合函数

| 函数 | 作用 | 示例 |
|------|------|------|
| **COUNT()** | 统计行数 | `COUNT(*)` 统计总行数 |
| **SUM()** | 求和 | `SUM(balance)` 余额总和 |
| **AVG()** | 平均值 | `AVG(age)` 平均年龄 |
| **MAX()** | 最大值 | `MAX(balance)` 最高余额 |
| **MIN()** | 最小值 | `MIN(created_at)` 最早注册时间 |

### 1.2 COUNT — 统计行数

```sql
-- COUNT(*)：统计所有行（包括 NULL）
SELECT COUNT(*) FROM users;

-- COUNT(字段名)：统计该字段非 NULL 的行数
SELECT COUNT(phone) FROM users;       -- 只统计填了手机号的用户
SELECT COUNT(*) FROM users;           -- 统计所有用户

-- 两者的区别：
-- 假设 users 表有 100 行，其中 20 行的 phone 为 NULL
SELECT COUNT(*) FROM users;           -- 100
SELECT COUNT(phone) FROM users;       -- 80
```

> ⚠️ `COUNT(*)` 和 `COUNT(字段)` 的区别是面试经典题。`COUNT(*)` 统计所有行，`COUNT(字段)` 忽略 NULL。

```sql
-- COUNT(DISTINCT 字段)：去重统计
SELECT COUNT(DISTINCT status) FROM users;  -- 有多少种不同的状态值
```

### 1.3 SUM / AVG / MAX / MIN

```sql
-- 余额总和
SELECT SUM(balance) FROM users;

-- 平均年龄
SELECT AVG(age) FROM users;

-- 最高余额
SELECT MAX(balance) FROM users;

-- 最早注册时间
SELECT MIN(created_at) FROM users;

-- 组合使用：一条 SQL 查多个聚合
SELECT
    COUNT(*)        AS 用户总数,
    SUM(balance)    AS 余额总和,
    AVG(balance)    AS 平均余额,
    MAX(balance)    AS 最高余额,
    MIN(balance)    AS 最低余额
FROM users;
```

> 📌 `AS` 是别名关键字，让结果列名更可读。实战中聚合查询基本都会加别名。

### 1.4 🔴 聚合函数的 NULL 处理

```sql
-- SUM/AVG/MAX/MIN 自动忽略 NULL
-- 但 COUNT(字段) 也忽略 NULL，导致"平均值对但总数不对"

-- 示例：10 个用户，2 个没余额（balance = NULL）
SELECT COUNT(*), AVG(balance) FROM users;   -- 10, AVG 基于 8 个值算
SELECT COUNT(balance), AVG(balance) FROM users; -- 8, AVG 同样基于 8 个值

-- IFNULL / COALESCE 处理 NULL
SELECT AVG(IFNULL(balance, 0)) FROM users;  -- 把 NULL 当 0 算平均值
```

> 🎯 **测试启示**：验证聚合结果时，要注意 NULL 值是否被正确处理。一个常见的 Bug 是平均值计算忽略了 NULL，导致结果"看起来对但实际偏高/偏低"。

---

## 二、GROUP BY — 分组统计

`GROUP BY` 将数据按指定字段分组，然后对每组使用聚合函数。

### 2.1 基本语法

```sql
SELECT 分组字段, 聚合函数(统计字段)
FROM 表名
WHERE 条件
GROUP BY 分组字段;
```

### 2.2 实战示例

```sql
-- 按状态统计用户数
SELECT
    status,
    COUNT(*) AS 用户数
FROM users
GROUP BY status;

-- 结果示例：
-- | status | 用户数 |
-- |--------|--------|
-- | 0      | 15     |  ← 禁用用户 15 个
-- | 1      | 85     |  ← 正常用户 85 个

-- 按日期统计注册人数
SELECT
    DATE(created_at) AS 注册日期,
    COUNT(*)         AS 注册人数
FROM users
GROUP BY DATE(created_at)
ORDER BY 注册日期 DESC;
```

### 2.3 多字段分组

```sql
-- 按状态 + 年龄分组
SELECT
    status,
    CASE
        WHEN age < 18 THEN '未成年'
        WHEN age BETWEEN 18 AND 30 THEN '青年'
        WHEN age BETWEEN 31 AND 50 THEN '中年'
        ELSE '老年'
    END AS 年龄段,
    COUNT(*) AS 人数
FROM users
GROUP BY status, 年龄段
ORDER BY status, 人数 DESC;
```

### 2.4 GROUP BY 的核心规则

> ⚠️ **重要**：SELECT 中出现的非聚合字段，**必须**出现在 GROUP BY 中。

```sql
-- ❌ 错误：username 不在 GROUP BY 中
SELECT username, status, COUNT(*) FROM users GROUP BY status;

-- ✅ 正确：SELECT 的非聚合字段都在 GROUP BY 中
SELECT status, COUNT(*) FROM users GROUP BY status;
```

### 2.5 测试场景：GROUP BY 实战

```sql
-- 统计各状态用户的平均余额
SELECT
    status,
    COUNT(*)    AS 用户数,
    AVG(balance) AS 平均余额,
    SUM(balance) AS 余额总和
FROM users
GROUP BY status;

-- 按小时统计注册量（找高峰期）
SELECT
    HOUR(created_at) AS 小时,
    COUNT(*)         AS 注册量
FROM users
GROUP BY HOUR(created_at)
ORDER BY 小时;

-- 统计每天各状态的用户数（双维度交叉）
SELECT
    DATE(created_at) AS 日期,
    status,
    COUNT(*)         AS 人数
FROM users
GROUP BY DATE(created_at), status
ORDER BY 日期 DESC, status;
```

---

## 三、HAVING — 分组后的过滤器

### 3.1 WHERE vs HAVING

> ⚠️ **面试高频考点**

| | WHERE | HAVING |
|------|-------|--------|
| **过滤时机** | 分组**前**（对原始行过滤） | 分组**后**（对分组结果过滤） |
| **能用聚合函数吗** | ❌ 不能 | ✅ 能 |
| **执行顺序** | FROM → **WHERE** → GROUP BY → **HAVING** → SELECT → ORDER BY |

```sql
-- WHERE：过滤原始行（不能用聚合函数）
SELECT status, COUNT(*) AS cnt
FROM users
WHERE age > 18          -- 先过滤：只要 18 岁以上的
GROUP BY status;

-- HAVING：过滤分组结果（能用聚合函数）
SELECT status, COUNT(*) AS cnt
FROM users
GROUP BY status
HAVING COUNT(*) > 10;   -- 再过滤：只要人数超过 10 的组
```

### 3.2 HAVING 实战

```sql
-- 找出注册用户超过 100 的日期
SELECT
    DATE(created_at) AS 日期,
    COUNT(*)         AS 注册数
FROM users
GROUP BY DATE(created_at)
HAVING COUNT(*) > 100
ORDER BY 日期;

-- 找出余额总和超过 10000 的用户状态组
SELECT
    status,
    COUNT(*)    AS 用户数,
    SUM(balance) AS 余额总和
FROM users
GROUP BY status
HAVING SUM(balance) > 10000;

-- WHERE + HAVING 组合
SELECT
    DATE(created_at) AS 日期,
    COUNT(*)         AS 注册数
FROM users
WHERE status = 1        -- ① 先过滤：只要正常用户
GROUP BY DATE(created_at) -- ② 再分组：按日期
HAVING COUNT(*) >= 5;    -- ③ 再过滤：只要注册 ≥5 的日期
```

### 3.3 经典面试题：WHERE 和 HAVING 能互换吗？

```sql
-- 这两个查询结果一样吗？

-- 写法 1：WHERE
SELECT status, COUNT(*) FROM users WHERE status = 1 GROUP BY status;

-- 写法 2：HAVING
SELECT status, COUNT(*) FROM users GROUP BY status HAVING status = 1;
```

> ✅ 结果一样，但**性能不同**：WHERE 在分组前过滤，数据量小了再分组，**更快**。能用 WHERE 就不要用 HAVING。

---

## 四、完整执行顺序（面试必考）

```sql
SELECT  字段列表, 聚合函数          -- ⑤ 选择最终返回的列
FROM    表名                        -- ① 确定数据来自哪张表
WHERE   原始行过滤条件               -- ② 过滤原始行
GROUP BY 分组字段                    -- ③ 按字段分组
HAVING  分组后过滤条件               -- ④ 过滤分组结果
ORDER BY 排序字段                    -- ⑥ 排序
LIMIT   行数;                       -- ⑦ 限制返回行数
```

> 🎯 **记忆口诀**：**F**rom **W**here **G**roup **H**aving **S**elect **O**rder **L**imit → "FWGHSOL"

---

## 五、常用字符串函数 — 测试数据处理利器

### 5.1 字符串拼接与截取

```sql
-- CONCAT：拼接字符串
SELECT CONCAT(username, ' <', email, '>') AS 用户信息 FROM users;
-- 结果：zane <zane@test.com>

-- CONCAT_WS：用分隔符拼接（更推荐）
SELECT CONCAT_WS(' | ', username, email, phone) AS 联系方式 FROM users;
-- 结果：zane | zane@test.com | 13800138000

-- SUBSTRING：截取字符串
SELECT SUBSTRING(username, 1, 3) FROM users;  -- 取前 3 个字符
SELECT SUBSTRING(phone, 1, 3) FROM users;     -- 手机号前 3 位（号段）

-- LENGTH / CHAR_LENGTH：长度
SELECT username, CHAR_LENGTH(username) AS 字符数 FROM users;
```

### 5.2 大小写与去空格

```sql
-- UPPER / LOWER：大小写转换
SELECT UPPER(username) FROM users;   -- 转大写
SELECT LOWER(email) FROM users;      -- 转小写

-- TRIM：去首尾空格（数据清洗常用）
SELECT TRIM(username) FROM users;
SELECT LTRIM(username) FROM users;   -- 只去左边空格
SELECT RTRIM(username) FROM users;   -- 只去右边空格
```

### 5.3 替换与定位

```sql
-- REPLACE：替换字符串
SELECT REPLACE(email, '@test.com', '@real.com') FROM users;

-- 数据脱敏：手机号中间 4 位变 ****
SELECT
    username,
    CONCAT(SUBSTRING(phone, 1, 3), '****', SUBSTRING(phone, 8, 4)) AS 脱敏手机号
FROM users;
-- 结果：138****8000

-- LOCATE：查找子串位置
SELECT email, LOCATE('@', email) FROM users;  -- @ 在第几个字符
```

### 5.4 测试场景：字符串函数实战

```sql
-- 验证：手机号是否是 11 位纯数字
SELECT phone
FROM users
WHERE phone IS NOT NULL
  AND (CHAR_LENGTH(phone) != 11 OR phone NOT REGEXP '^[0-9]+$');

-- 验证：邮箱格式是否正确
SELECT email
FROM users
WHERE email IS NOT NULL
  AND email NOT LIKE '%@%.%';

-- 数据脱敏查询（测试环境避免泄露真实数据）
SELECT
    id,
    CONCAT(SUBSTRING(username, 1, 1), '***') AS 用户名,
    CONCAT(SUBSTRING(phone, 1, 3), '****', SUBSTRING(phone, 8, 4)) AS 手机号
FROM users;
```

---

## 📝 今日练习

### 练习一：聚合函数基础

假设 `users` 表已有数据，写出以下 SQL：

| 需求 | 你的答案 |
|------|----------|
| 统计用户总数 | |
| 计算所有用户的平均余额 | |
| 找出余额最高的用户的余额值 | |
| 统计填了手机号的用户数（提示：phone IS NOT NULL） | |
| 统计有多少种不同的 status 值 | |

### 练习二：GROUP BY 分组统计

| 需求 | 你的答案 |
|------|----------|
| 按 status 分组统计各状态的用户数 | |
| 按日期分组统计每天注册的用户数 | |
| 按 status 分组统计各状态的平均余额和最高余额 | |
| 找出余额总和最高的那个 status 组（提示：ORDER BY + LIMIT） | |

### 练习三：HAVING 过滤

| 需求 | 你的答案 |
|------|----------|
| 找出用户数超过 10 的 status 组 | |
| 找出平均余额低于 50 的 status 组 | |
| 找出注册用户数超过 5 的日期 | |

### 练习四：WHERE vs HAVING

下面两个 SQL 的区别是什么？哪个更好？

```sql
-- A
SELECT status, COUNT(*) FROM users WHERE status = 1 GROUP BY status;

-- B
SELECT status, COUNT(*) FROM users GROUP BY status HAVING status = 1;
```

### 练习五：测试分析综合题

你负责测试一个电商平台的用户模块。PM 给你以下需求，请写出对应的验证 SQL：

1. 验证"每日新增用户数"报表：查询昨天注册的用户数
2. 验证"用户余额分布"：统计余额在 0-100、100-500、500-1000、1000+ 四个区间的用户数
3. 数据质量检查：找出手机号不是 11 位或包含非数字字符的用户
4. 异常检测：找出余额为负数或创建时间在未来的"脏数据"

---

## 📋 自检清单

- [ ] 五大聚合函数（COUNT/SUM/AVG/MAX/MIN）各是做什么的？
- [ ] `COUNT(*)` 和 `COUNT(字段)` 的区别是什么？
- [ ] 聚合函数如何处理 NULL 值？
- [ ] GROUP BY 的作用是什么？SELECT 中非聚合字段必须满足什么条件？
- [ ] WHERE 和 HAVING 的区别是什么（执行时机、能否用聚合函数）？
- [ ] 完整的 SELECT 执行顺序能默写吗？（FWGHSOL）
- [ ] 为什么能用 WHERE 就不要用 HAVING？
- [ ] CONCAT / SUBSTRING / REPLACE / TRIM 分别是做什么的？
- [ ] 怎么用 CONCAT + SUBSTRING 做手机号脱敏？
- [ ] 怎么用 SQL 做数据质量检查（邮箱格式、手机号长度）？

---

> 🎯 **今日小结**：第三天我们学会了用 SQL 做数据分析——聚合函数把数据"压缩"成统计指标，GROUP BY 把数据"切开"分组看，HAVING 在分组结果上再加过滤器。这些是测试工程师写测试报告、做数据验证的核心技能。字符串函数则让你能灵活处理测试中的各种文本数据。
>
> **第四天预告**：多表查询 — JOIN（INNER / LEFT / RIGHT），把分散在多个表里的数据关联起来，一次性查出完整信息！

---

# 第四天：多表查询 — JOIN 联表与 UNION 合并

> 📅 学习日期：第四天 | 🎯 模块：MySQL 数据库 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：为什么需要多表查询？

前三天我们一直在操作单张 `users` 表。但真实系统中，数据分散在多张表中：

```
电商系统：用户表、商品表、订单表、订单明细表
社交系统：用户表、帖子表、评论表、关注关系表
金融系统：账户表、交易流水表、对账表
```

> 🎯 **一句话**：单表查询只能看到"局部"，JOIN 才能看到"全局"。测试工程师验证业务流程时，几乎每次都要联表查。

---

## 一、先建一个电商迷你数据库

在学 JOIN 之前，我们先建好 4 张关联表。后面所有 JOIN 示例都基于这个 schema。

### 1.1 建表

```sql
-- 用户表（已有，简化版）
CREATE TABLE users (
    id       INT PRIMARY KEY AUTO_INCREMENT,
    username VARCHAR(50) NOT NULL,
    balance  DECIMAL(10,2) DEFAULT 0.00
);

-- 商品表
CREATE TABLE products (
    id    INT PRIMARY KEY AUTO_INCREMENT,
    name  VARCHAR(100) NOT NULL,
    price DECIMAL(10,2) NOT NULL,
    stock INT NOT NULL DEFAULT 0
);

-- 订单表（一个用户可以有多个订单 → 一对多）
CREATE TABLE orders (
    id          INT PRIMARY KEY AUTO_INCREMENT,
    user_id     INT NOT NULL,              -- 外键：关联 users.id
    total_amount DECIMAL(10,2) NOT NULL,
    status      TINYINT DEFAULT 1,         -- 1=待支付 2=已支付 3=已发货 4=已完成
    created_at  DATETIME DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id)
);

-- 订单明细表（一个订单可以有多个商品 → 一对多）
CREATE TABLE order_items (
    id         INT PRIMARY KEY AUTO_INCREMENT,
    order_id   INT NOT NULL,               -- 外键：关联 orders.id
    product_id INT NOT NULL,               -- 外键：关联 products.id
    quantity   INT NOT NULL,
    price      DECIMAL(10,2) NOT NULL,     -- 下单时的单价（防止后续商品改价影响历史订单）
    FOREIGN KEY (order_id) REFERENCES orders(id),
    FOREIGN KEY (product_id) REFERENCES products(id)
);
```

### 1.2 插入测试数据

```sql
-- 用户
INSERT INTO users (username, balance) VALUES
('张三', 1000.00), ('李四', 500.00), ('王五', 200.00), ('赵六', 0.00);

-- 商品
INSERT INTO products (name, price, stock) VALUES
('机械键盘', 299.00, 50), ('无线鼠标', 99.00, 200), ('显示器', 1299.00, 30);

-- 订单（张三 2 笔，李四 1 笔，赵六 0 笔）
INSERT INTO orders (user_id, total_amount, status, created_at) VALUES
(1, 398.00, 4, '2026-07-01 10:00:00'),  -- 张三 订单1
(1, 1299.00, 2, '2026-07-15 14:00:00'), -- 张三 订单2
(2, 99.00, 3, '2026-07-20 09:00:00');   -- 李四 订单1

-- 订单明细
INSERT INTO order_items (order_id, product_id, quantity, price) VALUES
(1, 1, 1, 299.00),  -- 订单1：机械键盘 ×1
(1, 2, 1, 99.00),   -- 订单1：无线鼠标 ×1
(2, 3, 1, 1299.00), -- 订单2：显示器 ×1
(3, 2, 1, 99.00);   -- 订单3：无线鼠标 ×1
```

### 1.3 四张表的 ER 关系

```
  users               orders              order_items         products
┌──────────┐      ┌──────────────┐      ┌──────────────┐      ┌──────────┐
│ id (PK)  │──┐   │ id (PK)      │──┐   │ id (PK)      │      │ id (PK)  │
│ username │  │   │ user_id (FK) │  │   │ order_id (FK)│──┐   │ name     │
│ balance  │  └──>│ total_amount │  └──>│ product_id(FK)│  └──>│ price    │
└──────────┘      │ status       │      │ quantity     │      │ stock    │
                  │ created_at   │      │ price        │      └──────────┘
                  └──────────────┘      └──────────────┘
```

---

## 二、INNER JOIN — 内连接（取交集）

### 2.1 什么是 INNER JOIN？

> `INNER JOIN` 只返回**两张表中匹配上的行**。不匹配的行被丢弃。

```
  左表         右表
┌──────┐    ┌──────┐
│  A   │    │  B   │
│  B   │────│  B   │  ← 匹配
│  C   │    │  D   │
└──────┘    └──────┘
  结果：只有 B 这一行
```

### 2.2 基本语法

```sql
SELECT 字段列表
FROM 表1
INNER JOIN 表2 ON 表1.关联字段 = 表2.关联字段;
```

### 2.3 实战示例

```sql
-- 查询每个订单的买家用户名
SELECT orders.id, orders.total_amount, users.username
FROM orders
INNER JOIN users ON orders.user_id = users.id;

-- 结果：
-- | id | total_amount | username |
-- |----|------------- |----------|
-- | 1  | 398.00       | 张三     |
-- | 2  | 1299.00      | 张三     |
-- | 3  | 99.00        | 李四     |
-- 🚫 赵六没有订单，不会出现在结果中
```

### 2.4 三表 JOIN

```sql
-- 查询每个订单的买家 + 买了什么商品
SELECT
    o.id          AS 订单号,
    u.username    AS 买家,
    p.name        AS 商品,
    oi.quantity   AS 数量,
    oi.price      AS 单价
FROM orders o
INNER JOIN users u        ON o.user_id = u.id
INNER JOIN order_items oi ON o.id = oi.order_id
INNER JOIN products p     ON oi.product_id = p.id;

-- 结果：
-- | 订单号 | 买家 | 商品     | 数量 | 单价   |
-- |--------|------|----------|------|--------|
-- | 1      | 张三 | 机械键盘  | 1    | 299.00 |
-- | 1      | 张三 | 无线鼠标  | 1    | 99.00  |
-- | 2      | 张三 | 显示器    | 1    | 1299.00|
-- | 3      | 李四 | 无线鼠标  | 1    | 99.00  |
```

> 📌 `o`/`u`/`p`/`oi` 是**表的别名**，让 SQL 更简洁。`orders o` = `orders AS o`（AS 可省略）。

### 2.5 JOIN + WHERE + GROUP BY 组合

```sql
-- 统计每个用户的订单数和总消费（含 0 单用户？不行，INNER JOIN 会排除）
SELECT
    u.username,
    COUNT(o.id)         AS 订单数,
    IFNULL(SUM(o.total_amount), 0) AS 总消费
FROM users u
INNER JOIN orders o ON u.id = o.user_id
WHERE o.status != 1     -- 排除已取消的订单
GROUP BY u.id, u.username
HAVING COUNT(o.id) >= 2 -- 只要下单 2 次以上的
ORDER BY 总消费 DESC;
```

---

## 三、LEFT JOIN — 左连接（保留左表全部）

### 3.1 什么是 LEFT JOIN？

> `LEFT JOIN` 返回**左表的所有行**，右表匹配不上则填 NULL。

```
  左表         右表
┌──────┐    ┌──────┐
│  A   │    │  B   │
│  B   │────│  B   │  ← 匹配
│  C   │    │  D   │  ← C 在右表中没有匹配 → 右表字段填 NULL
└──────┘    └──────┘
  结果：A(NULL) + B(NOT NULL) + C(NULL)
```

### 3.2 实战示例

```sql
-- 查询所有用户及其订单（含没下过单的赵六）
SELECT
    u.username,
    o.id       AS 订单号,
    o.total_amount
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

-- 结果：
-- | username | 订单号 | total_amount |
-- |----------|--------|-------------|
-- | 张三     | 1      | 398.00      |
-- | 张三     | 2      | 1299.00     |
-- | 李四     | 3      | 99.00       |
-- | 王五     | NULL   | NULL        |  ← 有用户但没订单
-- | 赵六     | NULL   | NULL        |  ← 有用户但没订单
```

### 3.3 测试场景：找出没下过单的用户

```sql
-- 方法一：LEFT JOIN + IS NULL
SELECT u.username
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.id IS NULL;     -- 右表没有匹配的行

-- 结果：王五、赵六
```

> 🎯 这是测试中**数据完整性检查**的经典用法："找出 A 表中存在但 B 表中没有对应记录的数据"。

### 3.4 更多测试场景

```sql
-- 找出从未被购买的商品
SELECT p.name
FROM products p
LEFT JOIN order_items oi ON p.id = oi.product_id
WHERE oi.id IS NULL;

-- 统计每个用户的下单情况（包含 0 单用户）
SELECT
    u.username,
    COUNT(o.id) AS 订单数,
    IFNULL(SUM(o.total_amount), 0) AS 总消费
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.id, u.username;
-- 🆚 对比 INNER JOIN：INNER JOIN 会直接排除赵六和王五
```

---

## 四、RIGHT JOIN — 右连接（保留右表全部）

### 4.1 什么是 RIGHT JOIN？

> `RIGHT JOIN` 返回**右表的所有行**，左表匹配不上则填 NULL。

本质上和 LEFT JOIN 是镜像关系：`A LEFT JOIN B` = `B RIGHT JOIN A`。

```sql
-- 这两条 SQL 完全等价
SELECT * FROM users u LEFT JOIN orders o ON u.id = o.user_id;
SELECT * FROM orders o RIGHT JOIN users u ON u.id = o.user_id;
```

> 📌 实际开发中 **LEFT JOIN 用得最多**（90%+），RIGHT JOIN 很少直接使用——把所有需要的表放左边更直观。

### 4.2 什么时候用 RIGHT JOIN？

当你已经写了一长串 JOIN，突然需要保留最后那张表的所有行时：

```sql
-- 查所有商品及其销售情况（含从未被买过的商品）
SELECT p.name, oi.quantity, oi.price
FROM order_items oi
RIGHT JOIN products p ON oi.product_id = p.id;
-- 这样写比从头 LEFT JOIN 改起方便
```

---

## 五、JOIN 对比总结

> ⚠️ **面试高频考点**

| JOIN 类型 | 返回结果 | 通俗类比 |
|-----------|---------|----------|
| **INNER JOIN** | 两张表都有的行（交集） | 双方都同意的匹配 |
| **LEFT JOIN** | 左表全部 + 右表匹配的（左全） | 以左表为准，右表来补充 |
| **RIGHT JOIN** | 右表全部 + 左表匹配的（右全） | 以右表为准，左表来补充 |
| **CROSS JOIN** | 笛卡尔积（每行×每行） | 所有可能的组合 |
| **FULL JOIN** | 两张表全部行（并集） | MySQL 不直接支持，用 UNION 替代 |

### 图解

```
INNER JOIN:         LEFT JOIN:           RIGHT JOIN:
  A ∩ B               A (全部)              B (全部)
┌──┬──┐            ┌──┬────┐            ┌────┬──┐
│✓ │  │            │✓ │ ✓  │            │ ✓  │✓ │
├──┼──┤            ├──┼────┤            ├────┼──┤
│  │  │            │✓ │ ✓  │            │ ✓  │✓ │
└──┴──┘            └──┴────┘            └────┴──┘
```

---

## 六、JOIN 的常见误区与面试陷阱

### 6.1 ON 和 WHERE 的区别

```sql
-- INNER JOIN 中：ON 和 WHERE 效果一样（但不代表可以乱写）
SELECT * FROM users u
INNER JOIN orders o ON u.id = o.user_id AND o.status = 1;  -- ✅

SELECT * FROM users u
INNER JOIN orders o ON u.id = o.user_id WHERE o.status = 1; -- ✅ 结果相同

-- LEFT JOIN 中：ON 和 WHERE 完全不同！
SELECT * FROM users u
LEFT JOIN orders o ON u.id = o.user_id AND o.status = 1;
-- 结果：所有用户都在，但只匹配 status=1 的订单（其他订单变 NULL）

SELECT * FROM users u
LEFT JOIN orders o ON u.id = o.user_id WHERE o.status = 1;
-- 结果：只有 status=1 订单的用户，退化为 INNER JOIN 效果！
```

> 🔴 **LEFT JOIN 中 WHERE 对右表字段的过滤会让 LEFT JOIN 退化为 INNER JOIN！** 因为 NULL 不满足任何 WHERE 条件。

### 6.2 JOIN 性能问题

```sql
-- ❌ 差：JOIN 没有索引的字段
SELECT * FROM orders o JOIN users u ON o.user_id = u.id;
-- 如果 orders.user_id 没有索引，每行都要全表扫描 users

-- ✅ 好：外键字段建索引（第一天建表时 FOREIGN KEY 已自动创建索引）
```

---

## 七、UNION / UNION ALL — 纵向合并

### 7.1 概念

JOIN 是**横向**扩展（加列），UNION 是**纵向**合并（加行）。

```sql
-- UNION：合并两个查询结果，自动去重
SELECT username FROM users WHERE balance > 500
UNION
SELECT username FROM users WHERE status = 1;

-- UNION ALL：合并两个查询结果，不去重（更快）
SELECT username FROM users WHERE balance > 500
UNION ALL
SELECT username FROM users WHERE status = 1;
```

### 7.2 UNION 规则

1. 两个查询的**列数**必须相同
2. 对应列的**数据类型**必须兼容
3. 结果集的列名以**第一个查询**为准

### 7.3 测试场景

```sql
-- 构造"全量测试数据 + 生产采样数据"用于测试
SELECT username, email FROM test_users
UNION ALL
SELECT username, email FROM prod_users WHERE id <= 100;

-- 找出"有余额但没下过单"和"下过单但余额为 0"的用户（两类异常用户）
SELECT username, '有钱但没买过' AS 异常类型 FROM users
WHERE balance > 0 AND id NOT IN (SELECT DISTINCT user_id FROM orders)
UNION
SELECT username, '买过但没钱了' AS 异常类型 FROM users
WHERE balance = 0 AND id IN (SELECT DISTINCT user_id FROM orders);
```

---

## 📝 今日练习

### 练习一：JOIN 概念判断

判断以下说法是否正确：

1. INNER JOIN 的结果行数一定 ≤ 左表行数
2. LEFT JOIN 的结果行数一定 = 左表行数
3. LEFT JOIN + WHERE 右表字段 IS NULL 可以找出"左表有但右表没有"的数据
4. `A LEFT JOIN B` 等价于 `B RIGHT JOIN A`

### 练习二：写 JOIN 查询

基于上方的电商 schema（users / orders / order_items / products），写出以下 SQL：

| 需求 | 你的答案 |
|------|----------|
| 查询所有订单的买家用户名和订单金额 | |
| 查询所有用户（含没下过单的），列出用户名和订单数 | |
| 查询每个商品的被购买次数（含没被买过的） | |
| 查询"订单号、买家、商品名、数量、单价"（三表 JOIN） | |
| 找出没下过单的用户（两种方法） | |

### 练习三：LEFT JOIN 陷阱分析

```sql
-- 这两条 SQL 结果为什么不同？
-- A
SELECT u.username, COUNT(o.id)
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
GROUP BY u.username;

-- B
SELECT u.username, COUNT(o.id)
FROM users u
LEFT JOIN orders o ON u.id = o.user_id AND o.status = 1
GROUP BY u.username;

-- C
SELECT u.username, COUNT(o.id)
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE o.status = 1
GROUP BY u.username;
```

### 练习四：测试分析综合题

你负责测试订单系统的数据一致性。写出 SQL：

1. 找出 `orders.total_amount` 和 `order_items` 中各项金额之和不一致的订单
2. 找出订单中的商品在下单后改了价格的记录（order_items.price ≠ products.price）
3. 找出库存为负数或小于已售数量的商品

---

## 📋 自检清单

- [ ] INNER JOIN / LEFT JOIN / RIGHT JOIN 的区别能画图解释吗？
- [ ] LEFT JOIN 中 ON 和 WHERE 对右表字段过滤的区别是什么？
- [ ] 为什么 LEFT JOIN 最常用（90%+）？
- [ ] 如何用 LEFT JOIN + IS NULL 找出"A 有 B 没有"的数据？
- [ ] JOIN 时为什么要给外键字段建索引？
- [ ] UNION 和 UNION ALL 的区别是什么？
- [ ] UNION 和 JOIN 的区别是什么（横向 vs 纵向）？
- [ ] 能独立写出 3 表 JOIN 的查询吗？
- [ ] `SELECT * FROM A LEFT JOIN B` 的行数一定等于 A 的行数吗？什么时候多于 A？

---

> 🎯 **今日小结**：第四天掌握了数据库学习中最重要的技能之一——多表联查。INNER JOIN 取交集，LEFT JOIN 保留左表全部，RIGHT JOIN 保留右表全部。测试中最常用的是 LEFT JOIN（完整性检查）和 INNER JOIN（查询关联数据）。UNION 则提供了纵向合并的能力。加上第三天的聚合函数，你现在已经能用 SQL 回答"谁买了什么、花了多少钱、谁还没买"这类完整的业务问题了。
>
> **第五天预告**：子查询与索引原理 — SELECT 嵌套 + B+ 树 + EXPLAIN 执行计划，开始接触性能优化！

---

# 第五天：子查询与索引原理 — 让查询更聪明、更快速

> 📅 学习日期：第五天 | 🎯 模块：MySQL 数据库 | ⏱️ 建议学习时长：2.5-3 小时

---

## 前言：今天的两大主题

前四天我们学会了建表、增删改查、聚合统计、多表联查。今天进入两个进阶主题：

- **子查询**：让一条 SQL 的输出成为另一条 SQL 的输入，实现"嵌套查询"
- **索引 + EXPLAIN**：理解数据库"快"和"慢"的底层原理，学会分析查询性能

> 🎯 **一句话**：子查询让你用 SQL 解决复杂问题，索引让你写的 SQL 跑得快。

---

## 第一部分：子查询（Subquery）

### 一、什么是子查询？

**子查询**就是嵌套在另一个 SQL 语句中的 SELECT 查询。外层查询叫"主查询"，内层叫"子查询"。

```
主查询：SELECT ... FROM ... WHERE 字段 IN (
    子查询：SELECT ... FROM ...
)
```

### 1.1 子查询的三种位置

| 位置 | 用途 | 子查询能返回 |
|------|------|------------|
| **WHERE 后面** | 作为过滤条件 | 单值 / 列表 / 多列 |
| **FROM 后面** | 作为临时表（派生表） | 一个结果集（必须给别名） |
| **SELECT 后面** | 作为计算字段（标量子查询） | 单个值 |

### 1.2 WHERE 子查询（最常用）

```sql
-- 场景：查询下过单的所有用户
-- 普通方法：先查 orders 中的 user_id，再查 users
SELECT DISTINCT user_id FROM orders;                     -- ① 得到 1, 2
SELECT * FROM users WHERE id IN (1, 2);                  -- ② 手动填入

-- 子查询方法：一步到位
SELECT * FROM users
WHERE id IN (SELECT DISTINCT user_id FROM orders);

-- 场景：查询余额高于所有用户平均值的用户
SELECT * FROM users
WHERE balance > (SELECT AVG(balance) FROM users);

-- 场景：查询"消费金额最高的用户"
SELECT * FROM users
WHERE id = (
    SELECT user_id FROM orders
    GROUP BY user_id
    ORDER BY SUM(total_amount) DESC
    LIMIT 1
);
```

### 1.3 子查询运算符

```sql
-- = / > / < / >= / <= ：子查询必须返回单个值
SELECT * FROM users WHERE balance = (SELECT MAX(balance) FROM users);

-- IN：子查询可以返回多个值
SELECT * FROM users WHERE id IN (SELECT user_id FROM orders);

-- NOT IN：排除子查询的值
SELECT * FROM users WHERE id NOT IN (SELECT user_id FROM orders);

-- EXISTS：子查询有结果就返回 True（只关心"有没有"，不关心"是什么"）
SELECT * FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);

-- NOT EXISTS：子查询没有结果时返回 True
SELECT * FROM users u
WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);
```

### 1.4 EXISTS vs IN 的区别

> ⚠️ **面试高频考点**

| | IN | EXISTS |
|------|-----|--------|
| **原理** | 先执行子查询，得到结果列表，再逐行比较 | 对主表每行，去子查询中找匹配 |
| **适合** | 子查询结果**小**时 | 主表结果**小**时 |
| **NULL 处理** | 子查询结果含 NULL 时可能全不匹配 | 不受 NULL 影响 |

```sql
-- IN：子查询先跑，得到列表 [1,2,3]，然后 WHERE id IN (1,2,3)
SELECT * FROM users WHERE id IN (SELECT user_id FROM orders);

-- EXISTS：对 users 每一行，检查 orders 中是否有 user_id = 当前行的 id
SELECT * FROM users u
WHERE EXISTS (SELECT 1 FROM orders o WHERE o.user_id = u.id);
```

> 📌 实际中 90% 的情况 IN 和 EXISTS 都能互换。面试重点是说清楚原理差异。

### 1.5 FROM 子查询（派生表）

```sql
-- 子查询作为临时表，必须起别名！
SELECT * FROM (
    SELECT user_id, COUNT(*) AS order_count
    FROM orders
    GROUP BY user_id
) AS user_orders    -- ← 别名必须有！
WHERE order_count >= 2;

-- 多表 JOIN 也可以和派生表结合
SELECT u.username, uo.order_count
FROM users u
LEFT JOIN (
    SELECT user_id, COUNT(*) AS order_count
    FROM orders
    GROUP BY user_id
) AS uo ON u.id = uo.user_id;
```

### 1.6 SELECT 子查询（标量子查询）

```sql
-- 每条用户记录旁显示"全站最高余额"
SELECT
    username,
    balance,
    (SELECT MAX(balance) FROM users) AS 全站最高,
    balance - (SELECT AVG(balance) FROM users) AS 与均值的差值
FROM users;
```

### 1.7 子查询 vs JOIN — 什么时候用哪个？

```sql
-- 需求：查出下过单的用户信息

-- 方法一：子查询
SELECT * FROM users WHERE id IN (SELECT DISTINCT user_id FROM orders);

-- 方法二：JOIN
SELECT DISTINCT u.* FROM users u
INNER JOIN orders o ON u.id = o.user_id;

-- 选哪个？
-- 子查询：逻辑更清晰，适合"判断是否存在"的场景
-- JOIN：性能通常更好（可以利用索引），且能同时取两表的字段
```

> 🎯 **经验法则**：只查主表数据用子查询（IN/EXISTS），需要同时查两张表的数据用 JOIN。

---

## 第二部分：索引原理

### 二、什么是索引？

**索引**是数据库中用于加速数据查找的数据结构，相当于书的**目录**。

```
没有索引：     扫描全书找"JOIN 关键字"→ 全表扫描，极慢
有索引：       翻到目录 → "JOIN 关键字在第 200 页"→ 直接跳到 200 页，极快
```

数据库默认使用 **B+ 树（B+ Tree）** 作为索引的数据结构。

### 2.1 索引的核心作用

| 作用 | 说明 |
|------|------|
| ⚡ **加速查询** | SELECT 的 WHERE/JOIN/ORDER BY 条件走索引，速度提升 100~10000 倍 |
| 🔒 **唯一约束** | UNIQUE 索引和主键索引自动保证数据唯一性 |
| 📊 **加速分组排序** | GROUP BY / ORDER BY 可以利用索引的有序性 |

> 🔴 索引的代价：**写入变慢**（每次 INSERT/UPDATE/DELETE 都要维护索引）+ **占用磁盘空间**。不能无脑建索引。

### 2.2 索引的类型

| 类型 | 关键字 | 特点 |
|------|--------|------|
| **主键索引** | `PRIMARY KEY` | 唯一、非空、每表只能有一个、聚簇索引 |
| **唯一索引** | `UNIQUE` | 值唯一、允许 NULL、可以有多个 |
| **普通索引** | `INDEX` / `KEY` | 加速查询，无唯一约束 |
| **复合索引** | `INDEX(a, b, c)` | 多列组合，遵循最左前缀原则 |
| **全文索引** | `FULLTEXT` | 用于文本搜索 |

```sql
-- 建表时创建索引
CREATE TABLE users (
    id       INT PRIMARY KEY,              -- 主键索引（自动创建）
    username VARCHAR(50) UNIQUE,           -- 唯一索引（自动创建）
    email    VARCHAR(100),
    age      INT,
    INDEX idx_email (email),              -- 普通索引
    INDEX idx_age_email (age, email)      -- 复合索引
);

-- 建表后添加索引
CREATE INDEX idx_username ON users(username);
CREATE UNIQUE INDEX idx_email ON users(email);

-- 删除索引
DROP INDEX idx_username ON users;
```

### 2.3 B+ 树索引原理（简述）

> 只需理解以下概念，不必深究实现细节：

```
                     [50 | 100]              ← 非叶子节点（只存索引键，不存数据）
                    /    |     \
              [10|30]  [60|80]  [120|150]    ← 非叶子节点
              /  |  \   /  |  \   /   |   \
            [1][15][35][55][70][90][110][140][180]  ← 叶子节点（存实际数据指针）
                                                    叶子之间由双向链表连接
```

| B+ 树特性 | 影响 |
|-----------|------|
| 所有数据在叶子节点 | 查询任何数据都经过相同层数 → 查询稳定 |
| 叶子节点有序 + 链表串联 | 范围查询（`BETWEEN` / `>` / `<`）极快 |
| 一个节点存多个键 | 树很"矮胖"，3~4 层就能存千万数据 |
| 非叶子节点只存键 | 节点能存更多索引 → 树更矮 → 磁盘 IO 更少 |

### 2.4 聚簇索引 vs 非聚簇索引

> ⚠️ **面试高频考点**

| | 聚簇索引（Clustered） | 非聚簇索引（Non-Clustered） |
|------|------|------|
| **本质** | 索引的叶子节点直接存**整行数据** | 索引的叶子节点存**主键值**（再回表查） |
| **每表数量** | 只能有 **1 个**（因为数据只能按一种顺序排） | 可以有多个 |
| **默认是谁** | **主键**（PRIMARY KEY） | 手动创建的普通索引/唯一索引 |
| **查找过程** | 查到索引即得数据（一步到位） | 查到索引后还得到聚簇索引查数据（回表） |
| **类比** | 字典按拼音排列，正文就是索引 | 字典的笔画索引 → 查到页码 → 再翻到正文 |

```
聚簇索引查找：
  B+树查主键 id=100 → 叶子节点直接拿到整行数据 → 结束 ✅

非聚簇索引查找（回表）：
  B+树查 username='zane' → 得到主键 id=100 → 到聚簇索引再查 id=100 → 拿到整行数据
  这个"再查"叫回表，多一次磁盘 IO
```

> 🎯 **一句话**：主键查询最快（聚簇索引），非主键查询可能需要回表（两次索引查找）。

### 2.5 复合索引与最左前缀原则

> ⚠️ **面试高频考点**

```sql
-- 创建复合索引
CREATE INDEX idx_a_b_c ON 表名 (字段a, 字段b, 字段c);
```

这个索引相当于你按 `(a, b, c)` 的顺序创建了一个排序：

```
索引 idx_a_b_c 生效的条件（最左前缀）：
✅ WHERE a = 1                          -- 使用 a 列
✅ WHERE a = 1 AND b = 2                -- 使用 a, b 列
✅ WHERE a = 1 AND b = 2 AND c = 3      -- 使用全部三列
✅ WHERE a = 1 AND c = 3                -- 使用 a 列（b 断了，c 用不上）
❌ WHERE b = 2                          -- 跳过了 a，索引完全失效！
❌ WHERE b = 2 AND c = 3                -- 跳过了 a，索引完全失效！
❌ WHERE a = 1 OR b = 2                 -- OR 导致索引失效
```

> 📌 **口诀**：复合索引像楼梯，必须从第一级开始走，中间跳级就走不下去了。

### 2.6 索引失效的常见场景

> ⚠️ **面试和工作双高频！**

| 场景 | 示例 | 原因 |
|------|------|------|
| **LIKE 前导模糊** | `WHERE name LIKE '%zane'` | 索引无法定位开头是什么 |
| **对索引列用函数** | `WHERE DATE(created_at) = '2026-07-01'` | 函数改变了索引值 |
| **对索引列做运算** | `WHERE age + 1 = 20` | 应改为 `WHERE age = 19` |
| **隐式类型转换** | `WHERE phone = 13800138000` (phone 是 CHAR) | 字符串和数字比较导致隐式转换 |
| **OR 连接** | `WHERE a = 1 OR b = 2`（a 有索引，b 没有） | OR 一侧没索引，全索引失效 |
| **NOT / != / <>** | `WHERE status != 1` | 索引只能找"是什么"，不能找"不是什么" |
| **IS NULL** | 大部分情况索引会失效 | 用默认值代替 NULL |
| **复合索引跳首列** | `WHERE b = 2`（索引是 (a,b)） | 违反最左前缀 |

```sql
-- ❌ 索引失效写法
SELECT * FROM users WHERE username LIKE '%zane';       -- 前导模糊
SELECT * FROM users WHERE DATE(created_at) = '2026-07-01';  -- 对列用函数
SELECT * FROM users WHERE age + 1 = 20;                -- 对列做运算

-- ✅ 索引生效改写
SELECT * FROM users WHERE username LIKE 'zane%';       -- 后置模糊 OK
SELECT * FROM users WHERE created_at >= '2026-07-01' AND created_at < '2026-07-02';  -- 等价于 DATE()
SELECT * FROM users WHERE age = 19;                    -- 运算移到等号右边
```

---

## 三、EXPLAIN — 看穿 SQL 的执行过程

### 3.1 什么是 EXPLAIN？

`EXPLAIN` 是 MySQL 提供的**查询分析工具**，让你在**不实际执行**的情况下看到数据库会怎么执行你的 SQL。

```sql
EXPLAIN SELECT * FROM users WHERE username = 'zane';
```

### 3.2 EXPLAIN 输出解读

| 关键字段 | 含义 | 好 vs 坏 |
|----------|------|----------|
| **type** | 访问类型 | `ALL`（全表扫描）→ `index` → `range` → `ref` → `eq_ref` → `const`（最优） |
| **key** | 实际使用的索引 | NULL = 没走索引（⚠️ 危险） |
| **rows** | 预估扫描行数 | 越小越好，几十万就是灾难 |
| **Extra** | 额外信息 | `Using filesort`（⚠️ 需优化）、`Using temporary`（⚠️ 需优化）、`Using index`（✅ 覆盖索引） |

### 3.3 type 字段（最重要！）

```
从差到好排列：

ALL          全表扫描               ⚫⚫⚫⚫⚫ 灾难！必须优化
index        全索引扫描             ⚫⚫⚫⚫   很慢
range        索引范围扫描           ⚫⚫     可接受
ref          非唯一索引查找         ⚫       不错
eq_ref       唯一索引查找（JOIN）   🟢      很好
const/system 主键/常量查找          🟢🟢    最优
```

```sql
-- type = ALL（全表扫描）：灾难！
EXPLAIN SELECT * FROM users WHERE balance > 100;  -- balance 没索引

-- type = ref：不错
EXPLAIN SELECT * FROM users WHERE username = 'zane';  -- username 有 UNIQUE 索引

-- type = const：最优！
EXPLAIN SELECT * FROM users WHERE id = 1;  -- id 是主键
```

### 3.4 测试工程师如何使用 EXPLAIN

```sql
-- ① 线上慢查询排查：找出没走索引的 SQL
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
-- 看 type 是不是 ALL，key 是不是 NULL

-- ② 索引验证：加了索引后确认是否生效
CREATE INDEX idx_user_id ON orders(user_id);
EXPLAIN SELECT * FROM orders WHERE user_id = 5;
-- 看 key 是不是 idx_user_id

-- ③ JOIN 性能分析：确保驱动表和被驱动表都走索引
EXPLAIN SELECT * FROM users u
INNER JOIN orders o ON u.id = o.user_id;
-- 看每行的 type，ref 以上才合格
```

---

## 📝 今日练习

### 练习一：子查询

基于电商 schema，写出以下 SQL：

| 需求 | 你的答案 |
|------|----------|
| 查询余额最高的用户信息 | |
| 查询下过单的用户（用 IN） | |
| 查询没下过单的用户（用 NOT EXISTS） | |
| 查询"买了机械键盘的所有用户"（嵌套子查询） | |
| 统计每个用户的下单次数（用 FROM 子查询 + LEFT JOIN） | |

### 练习二：索引判断

判断以下情况，索引是否会生效：

| SQL | 索引 | 是否生效 |
|-----|------|----------|
| `WHERE username LIKE 'zane%'` | idx_username | |
| `WHERE username LIKE '%zane'` | idx_username | |
| `WHERE DATE(created_at) = '2026-07-01'` | idx_created_at | |
| `WHERE age = 18` | idx_age | |
| `WHERE a = 1 AND b = 2` | idx_a_b(a, b) | |
| `WHERE b = 2` | idx_a_b(a, b) | |
| `WHERE a = 1 OR b = 2` | idx_a_b(a, b) | |

### 练习三：EXPLAIN 实战

写出会用 EXPLAIN 分析的 SQL，并预估 type 等级：

| 查询 | 预估 type | 原因 |
|------|-----------|------|
| `SELECT * FROM users WHERE id = 1` | | 主键等值查询 |
| `SELECT * FROM users WHERE username = 'zane'` | | 唯一索引等值查询 |
| `SELECT * FROM users WHERE balance > 100` | | balance 未建索引 |
| `SELECT * FROM users u JOIN orders o ON u.id = o.user_id` | u: ? o: ? | 主键 vs 外键索引 |

---

## 📋 自检清单

### 子查询部分

- [ ] 子查询可以放在 SQL 的哪三个位置？
- [ ] `IN` 和 `EXISTS` 的区别是什么？什么时候用哪个？
- [ ] FROM 子查询为什么必须起别名？
- [ ] 什么场景用子查询比 JOIN 更合适？反过来呢？

### 索引部分

- [ ] 索引的本质是什么？为什么能加速查询？
- [ ] 聚簇索引和非聚簇索引的区别是什么？为什么每表只能有一个聚簇索引？
- [ ] 什么是"回表"？如何避免回表？
- [ ] 复合索引的最左前缀原则是什么？能举例说明吗？
- [ ] 列举至少 5 种索引失效的场景
- [ ] 为什么 `LIKE '%keyword'` 不走索引而 `LIKE 'keyword%'` 走？
- [ ] 索引是越多越好吗？建索引有什么代价？

### EXPLAIN 部分

- [ ] EXPLAIN 的 type 字段从差到好怎么排列？
- [ ] `type=ALL` 意味着什么？应该怎么优化？
- [ ] `Extra` 中出现 `Using filesort` 或 `Using temporary` 说明什么？

---

> 🎯 **今日小结**：第五天接触了 MySQL 中最有"工程师感"的两个主题。子查询让你能写出嵌套逻辑的复杂 SQL——把查询结果当条件、当表、当字段值。索引原理让你理解数据库的快慢从何而来——B+ 树是底层、聚簇索引是核心、最左前缀是面试必考。EXPLAIN 则给你一双"透视眼"，能看到每条 SQL 的执行过程。测试工程师不需要成为 DBA，但会看 EXPLAIN、能判断索引是否生效，就是**高级测试工程师**和普通测试员的分水岭。
>
> **第六天预告**：事务管理 — ACID 特性、隔离级别、锁机制、MVCC，数据库最核心的"安全机制"！

---

# 第六天：事务管理 — 保证数据一致性的核心机制

> 📅 学习日期：第六天 | 🎯 模块：MySQL 数据库 | ⏱️ 建议学习时长：2.5-3 小时

---

## 前言：为什么需要事务？

先看一个经典的"灾难场景"：

```
用户张三给李四转账 100 元：

Step 1: UPDATE users SET balance = balance - 100 WHERE username = '张三';  ✅ 执行成功
Step 2: UPDATE users SET balance = balance + 100 WHERE username = '李四';  ❌ 执行失败！（数据库宕机）

结果：张三的 100 元被扣了，但李四没收到。
      100 元凭空消失了！这就是没有事务保护的后果。
```

> 🎯 **一句话**：事务就是把多个 SQL 操作**打包成一个不可分割的整体**——要么全部成功，要么全部失败回滚。就像转账：扣钱和加钱必须同时成功或同时撤销。

---

## 一、事务基础

### 1.1 什么是事务？

**事务（Transaction）** 是由一组 SQL 语句组成的逻辑工作单元，具有原子性——不可再分。

### 1.2 事务的基本操作

```sql
-- 开启事务
START TRANSACTION;
-- 或
BEGIN;

-- 执行一组 SQL
UPDATE users SET balance = balance - 100 WHERE username = '张三';
UPDATE users SET balance = balance + 100 WHERE username = '李四';

-- 如果一切正常：提交（永久保存）
COMMIT;

-- 如果出了任何问题：回滚（撤销所有修改）
ROLLBACK;
```

### 1.3 实战：转账事务

```sql
-- 正确的转账写法
START TRANSACTION;

-- ① 检查张三余额是否足够
SELECT balance FROM users WHERE username = '张三';  -- 假设返回 1000

-- ② 扣钱
UPDATE users SET balance = balance - 100 WHERE username = '张三';

-- ③ 加钱
UPDATE users SET balance = balance + 100 WHERE username = '李四';

-- ④ 验证结果（可选但推荐）
SELECT balance FROM users WHERE username IN ('张三', '李四');

-- ⑤ 确认无误，提交
COMMIT;

-- 如果任何一步出错，执行：
-- ROLLBACK;  -- 所有修改撤销，数据回到事务开始前的状态
```

### 1.4 事务的自动提交模式

```sql
-- MySQL 默认：每条 SQL 自动提交（相当于每条 SQL 都是一个事务）
-- 查看当前模式
SELECT @@autocommit;  -- 1 = 自动提交开启

-- 关闭自动提交（当前会话）
SET autocommit = 0;
-- 此后所有 SQL 都需手动 COMMIT 或 ROLLBACK

-- 恢复自动提交
SET autocommit = 1;
```

> 📌 InnoDB 引擎支持事务，MyISAM 引擎不支持。现在建表默认都是 InnoDB。

---

## 二、ACID 特性（面试必考）

> ⚠️ **面试最高频考点之一，必须逐字理解！**

| 特性 | 全称 | 含义 | 类比 |
|------|------|------|------|
| **A** 原子性 | Atomicity | 事务中的所有操作要么**全部成功**，要么**全部失败回滚**，不可分割 | 一荣俱荣，一损俱损 |
| **C** 一致性 | Consistency | 事务执行前后，数据从一个**合法状态**变到另一个合法状态 | 转账前后总金额不变 |
| **I** 隔离性 | Isolation | 并发事务之间**互不干扰**，每个事务感觉自己在独占数据库 | 多人同时转账互不影响 |
| **D** 持久性 | Durability | 事务一旦提交，数据**永久保存**，即使系统崩溃也不丢失 | 写入磁盘，断电不丢 |

### 2.1 各特性深入理解

**原子性（Atomicity）**：
```
转账事务：扣张三 100 + 加李四 100
            ↓
         ┌──────────┐
         │ 扣张三   │ → 失败 → ROLLBACK → 两条都撤销 ✅
         │ 加李四   │ → 失败 → ROLLBACK → 两条都撤销 ✅
         │ 全部成功 │ → COMMIT → 两条都生效 ✅
         └──────────┘
```

**一致性（Consistency）**：
```
事务前：张三 1000，李四 500，总和 1500
事务后：张三 900，  李四 600，总和 1500  ← 总金额不变，满足"守恒定律"
如果：  张三 900，  李四 500                ← 100 元失踪，一致性被破坏！
```

**隔离性（Isolation）**：
```
事务 A：查余额 → 看到 1000
事务 B：同时查余额 → 也看到 1000（A 的修改在提交前对 B 不可见）
事务 A：扣 100 → 余额变为 900
事务 B：扣 200 → 基于 1000 扣到 800（不会基于 A 的中间状态）
```

**持久性（Durability）**：
```
事务 COMMIT 后：
  即使立即断电、操作系统崩溃、数据库进程被杀
  → 重启后数据仍然存在，绝不会"丢失已提交的数据"
实现机制：redo log（重做日志）+ 磁盘写入
```

---

## 三、并发问题 — 多个事务同时跑的麻烦

> ⚠️ **面试高频考点**

当多个事务同时操作同一数据时，可能产生三类问题：

### 3.1 脏读（Dirty Read）

**读取到其他事务未提交的修改。**

```
时间线：
T1: START TRANSACTION → UPDATE 余额 1000→900 （未提交）
T2: START TRANSACTION → SELECT 余额 → 读到 900  ← 脏读！
T1: ROLLBACK → 余额恢复为 1000
T2: 基于 900 做决策 → 数据是错的！（因为 T1 回滚了）
```

### 3.2 不可重复读（Non-Repeatable Read）

**同一事务内，两次读取同一行数据，结果不同。**（其他事务在中间 UPDATE 并提交了）

```
时间线：
T1: START TRANSACTION → SELECT 余额 → 1000
T2: UPDATE 余额=900 → COMMIT
T1: SELECT 余额 → 900  ← 和第一次读到的不一样！
```

### 3.3 幻读（Phantom Read）

**同一事务内，两次查询同一条件，返回的行数不同。**（其他事务在中间 INSERT 并提交了）

```
时间线：
T1: START TRANSACTION → SELECT COUNT(*) WHERE status=1 → 100 行
T2: INSERT 新用户(status=1) → COMMIT
T1: SELECT COUNT(*) WHERE status=1 → 101 行  ← 凭空多了一行！
```

### 3.4 三者的关键区别

| 问题 | 读到了什么 | 操作类型 | 行数变化 |
|------|-----------|----------|----------|
| **脏读** | 未提交的修改 | UPDATE | 不变 |
| **不可重复读** | 已提交的修改 | UPDATE | 不变 |
| **幻读** | 已提交的新行 | INSERT/DELETE | **变了** |

---

## 四、隔离级别（Isolation Level）

### 4.1 四种隔离级别

> ⚠️ **面试必考，必须记住各级别解决什么问题！**

| 隔离级别 | 脏读 | 不可重复读 | 幻读 | 性能 | MySQL 默认 |
|----------|:--:|:--:|:--:|:--:|:--:|
| **READ UNCOMMITTED**（读未提交） | ❌ | ❌ | ❌ | 最快 | — |
| **READ COMMITTED**（读已提交） | ✅ | ❌ | ❌ | 快 | Oracle/PostgreSQL |
| **REPEATABLE READ**（可重复读） | ✅ | ✅ | ⚠️ 部分 | 中 | **MySQL InnoDB** |
| **SERIALIZABLE**（串行化） | ✅ | ✅ | ✅ | 最慢 | — |

```sql
-- 查看当前隔离级别
SELECT @@transaction_isolation;  -- MySQL 8.0+
SELECT @@tx_isolation;           -- MySQL 5.7

-- 设置隔离级别
SET SESSION TRANSACTION ISOLATION LEVEL READ COMMITTED;
SET SESSION TRANSACTION ISOLATION LEVEL REPEATABLE READ;
```

### 4.2 各级别通俗解释

**READ UNCOMMITTED** — "不设防"
> 事务可以读到别人还没提交的数据。性能最快但几乎没人用（太不安全）。

**READ COMMITTED** — "只看已提交的"
> 只能读到别人已经 COMMIT 的数据。解决了脏读，但不可重复读仍存在。

**REPEATABLE READ**（MySQL 默认）— "我看到的不变"
> 事务开始后，读到的数据始终一致，即使其他事务修改并提交了也不影响。解决了脏读和不可重复读。MySQL 通过 MVCC 还额外解决了大部分幻读。

**SERIALIZABLE** — "排队执行"
> 所有事务串行执行，完全没有并发问题，但性能极差。极少使用。

### 4.3 测试工程师如何验证隔离级别？

```sql
-- 终端 1：模拟事务 A
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
START TRANSACTION;
UPDATE users SET balance = 999 WHERE username = '张三';
-- 注意：此时不提交！

-- 终端 2：模拟事务 B（同时开另一个 mysql 客户端）
SET SESSION TRANSACTION ISOLATION LEVEL READ UNCOMMITTED;
SELECT balance FROM users WHERE username = '张三';
-- READ UNCOMMITTED → 读到 999（脏读！事务 A 还没提交）
-- READ COMMITTED   → 读到 1000（事务 A 未提交，看不到）

-- 终端 1：
ROLLBACK;  -- 事务 A 回滚
```

---

## 五、MVCC — MySQL 如何实现高并发

### 5.1 什么是 MVCC？

**MVCC（Multi-Version Concurrency Control，多版本并发控制）** 是 MySQL InnoDB 实现高并发的核心技术。

> 核心思想：**读操作不阻塞写操作，写操作不阻塞读操作**。每个事务看到的是数据的"快照"版本，而不是实时数据。

### 5.2 一句话理解 MVCC

```
传统方式（加锁）：    读的时候不能写，写的时候不能读 → 性能差
MVCC 方式（快照）：   每个人看到的是自己的"快照版本" → 读写不冲突 → 性能好
```

### 5.3 MVCC 如何工作（简化版）

```
隐藏列：
每行数据有两个隐藏列：
- DB_TRX_ID：最后一次修改这行的事务 ID
- DB_ROLL_PTR：回滚指针，指向旧版本

当 REPEATABLE READ 下执行 SELECT 时：
  事务看到的是 ← 事务开始前最后一次提交的版本
  正在修改但未提交的版本 ← 不可见
```

> 📌 测试工程师不需要深究 MVCC 的实现细节，但需要知道：**MySQL 的 REPEATABLE READ 隔离级别靠 MVCC 实现了"快照读"，避免了脏读和不可重复读，且性能远优于 SERIALIZABLE。**

---

## 六、锁机制基础

### 6.1 两种锁类型

| 锁类型 | 简称 | 作用 | 兼容性 |
|--------|------|------|--------|
| **共享锁** | S 锁（读锁） | 允许读，阻止写 | 和 S 锁兼容，和 X 锁互斥 |
| **排他锁** | X 锁（写锁） | 阻止任何其他锁 | 和 S/X 都互斥 |

### 6.2 加锁方式

```sql
-- 共享锁（读锁）：SELECT ... LOCK IN SHARE MODE（MySQL 8.0 改为 FOR SHARE）
SELECT * FROM users WHERE id = 1 FOR SHARE;

-- 排他锁（写锁）：SELECT ... FOR UPDATE
SELECT * FROM users WHERE id = 1 FOR UPDATE;

-- 普通 SELECT 不加锁（MVCC 快照读）
SELECT * FROM users WHERE id = 1;  -- 无锁
```

### 6.3 锁粒度

| 粒度 | 锁定范围 | 并发度 | 冲突概率 |
|------|---------|--------|----------|
| **表锁** | 整张表 | 低 | 高 |
| **行锁** | 特定行 | 高 | 低 |
| **间隙锁** | 行之间的间隙 | 中 | 中（防止幻读） |

> InnoDB 默认使用**行锁**，只有通过索引查找时才会加行锁，否则退化为表锁。

### 6.4 死锁（Deadlock）

```
事务 A：锁了 users 第 1 行，等第 2 行
事务 B：锁了 users 第 2 行，等第 1 行
        → 互相等待 → 死锁！

MySQL 处理：自动检测死锁 → 回滚其中一个事务 → 报错：
ERROR 1213 (40001): Deadlock found when trying to get lock
```

> 🎯 **测试场景**：并发测试时如果遇到 Deadlock 错误，不是 Bug——这是数据库的正常保护机制。需要检查的是：应用层是否有重试逻辑。

---

## 七、测试工程师的事务实战

### 7.1 测试转账功能的验证 SQL

```sql
-- 测试前的数据快照
SELECT username, balance FROM users WHERE username IN ('张三', '李四');
-- 张三: 1000, 李四: 500

-- 模拟转账（在测试脚本中执行）
START TRANSACTION;
UPDATE users SET balance = balance - 100 WHERE username = '张三';
UPDATE users SET balance = balance + 100 WHERE username = '李四';
COMMIT;

-- 测试后的验证
SELECT username, balance FROM users WHERE username IN ('张三', '李四');
-- 预期：张三: 900, 李四: 600

-- 总余额校验（一致性）
SELECT SUM(balance) FROM users;
-- 预期：总额和转账前一样
```

### 7.2 测试并发场景

```sql
-- 验证并发扣款：两个请求同时对张三扣 100
-- 终端 1：
START TRANSACTION;
UPDATE users SET balance = balance - 100 WHERE username = '张三' AND balance >= 100;
-- 等待片刻...

-- 终端 2（同时执行）：
START TRANSACTION;
UPDATE users SET balance = balance - 100 WHERE username = '张三' AND balance >= 100;
-- 如果余额只有 100，终端 2 应该更新 0 行（不会出现余额负值）

-- 终端 1：COMMIT;
-- 终端 2：COMMIT;
-- 验证最终余额是否正确
```

### 7.3 测试隔离级别的影响

```sql
-- 验证 REPEATABLE READ 下不可重复读是否被解决
-- 终端 1：
START TRANSACTION;
SELECT balance FROM users WHERE username = '张三';  -- 看到 1000

-- 终端 2：
UPDATE users SET balance = 2000 WHERE username = '张三';
COMMIT;

-- 终端 1（同一事务内再次查询）：
SELECT balance FROM users WHERE username = '张三';  -- 还是 1000！✅ 不可重复读被解决
COMMIT;
```

---

## 📝 今日练习

### 练习一：ACID 理解

用自己的话解释以下场景属于 ACID 的哪个特性被破坏或保证：

| 场景 | 属于哪个特性 |
|------|-------------|
| 转账扣了钱但没加上，钱消失了 | |
| 提交后数据库宕机，重启后数据还在 | |
| 两个人同时改同一行数据，一个人覆盖了另一个人的修改 | |
| 张三看到余额 1000，同时李四给张三转了 2000 但还没提交，张三看到的仍是 1000 | |

### 练习二：隔离级别填空

| 隔离级别 | 解决了脏读？ | 解决了不可重复读？ | 解决了幻读？ |
|----------|:----------:|:----------------:|:----------:|
| READ UNCOMMITTED | | | |
| READ COMMITTED | | | |
| REPEATABLE READ | | | |
| SERIALIZABLE | | | |

### 练习三：事务 SQL 编写

写一个完整的事务 SQL，实现"订单支付"功能：
1. 检查用户余额是否 ≥ 订单金额
2. 扣除用户余额
3. 修改订单状态为"已支付"
4. 如有任何一步失败，全部回滚

### 练习四：测试思维

你负责测试一个电商的秒杀功能。100 个库存，1000 人同时抢。请回答：

1. 如果没有事务保护，可能出现什么问题？
2. 用什么 SQL 保证"扣库存时不超卖"？（提示：WHERE 条件）
3. 测试时如何验证最终库存数量是否正确？

---

## 📋 自检清单

- [ ] 什么是事务？为什么转账需要事务保护？
- [ ] ACID 四个字母各代表什么？能各举一个违反的例子吗？
- [ ] COMMIT 和 ROLLBACK 的区别是什么？
- [ ] 脏读、不可重复读、幻读三者有什么区别？
- [ ] MySQL 默认的隔离级别是什么？解决了哪些并发问题？
- [ ] 四种隔离级别从低到高怎么排列？
- [ ] MVCC 的核心思想是什么（一句话）？
- [ ] 共享锁（S 锁）和排他锁（X 锁）的区别是什么？
- [ ] `SELECT ... FOR UPDATE` 加的是什么锁？什么时候用？
- [ ] 死锁是什么？MySQL 怎么处理死锁？
- [ ] 测试并发功能时，如何验证数据一致性？

---

> 🎯 **今日小结**：第六天学习了数据库最核心的"安全机制"——事务。ACID 是面试的金字招牌，必须逐字理解；四种隔离级别是并发控制的四种"防护等级"，MySQL 默认的 REPEATABLE READ + MVCC 在性能和安全之间取得了最好的平衡；锁机制则让你理解"为什么高并发下数据不会乱"。测试工程师理解事务后，就能设计出有效的并发测试场景，验证系统在高并发下的数据一致性。
>
> **第七天预告**：MySQL 模块总结 — 备份恢复 + 测试数据工厂 + 综合实战 + 下一模块（Python 基础）预告！

---

# 第七天：MySQL 模块收官 — 备份恢复、测试数据工厂与总结

> 📅 学习日期：第七天 | 🎯 模块：MySQL 数据库（模块收官） | ⏱️ 建议学习时长：2-3 小时

---

## 前言：从一个测试工程师的日常说起

```
早上 9:00：开发提交了新版本，需要搭建测试环境 → 从备份恢复数据库
上午 10:30：写自动化用例，需要构造 500 条测试订单 → 批量 SQL 生成
下午 2:00：用例跑完了，需要清理脏数据 → 恢复初始状态
下午 4:30：生产环境报错，需要导出数据排查 → 备份+查询分析
```

今天的内容覆盖测试工程师数据库操作的"最后一公里"——备份恢复、批量造数、环境清理、以及整个 MySQL 模块的体系化总结。

---

## 一、备份与恢复 — mysqldump

### 1.1 为什么测试工程师需要学会备份恢复？

| 场景 | 操作 |
|------|------|
| 🏗️ **搭建测试环境** | 从生产库脱敏导出 → 导入到测试库 |
| 🧪 **自动化测试前后** | 备份初始状态 → 跑用例 → 恢复初始状态 |
| 🐛 **问题排查** | 导出当前数据状态，发给开发分析 |
| 💾 **数据保护** | 修改数据库前先备份，改错了能恢复 |

### 1.2 mysqldump 导出

> ⚠️ `mysqldump` 是**命令行工具**，不是在 `mysql>` 里面执行的！需要在终端（bash/cmd）中运行。

```bash
# 导出整个数据库
mysqldump -u root -p test_study > backup.sql

# 导出指定表
mysqldump -u root -p test_study users orders > tables_backup.sql

# 只导出表结构（不要数据）
mysqldump -u root -p --no-data test_study > schema_only.sql

# 只导出数据（不要表结构）
mysqldump -u root -p --no-create-info test_study > data_only.sql

# 导出时加条件（只导出部分数据）
mysqldump -u root -p test_study users --where="status=1" > active_users.sql

# 导出时忽略某些表
mysqldump -u root -p test_study --ignore-table=test_study.logs > no_logs.sql

# 完整参数（推荐用于生产备份）
mysqldump -u root -p \
    --single-transaction \    # 事务一致性备份（不锁表）
    --routines \              # 包含存储过程和函数
    --triggers \              # 包含触发器
    --add-drop-database \     # 导入前先删库
    test_study > full_backup.sql
```

### 1.3 mysql 导入恢复

```bash
# 导入 SQL 文件
mysql -u root -p test_study < backup.sql

# 导入到新数据库
mysql -u root -p -e "CREATE DATABASE test_restore"
mysql -u root -p test_restore < backup.sql

# 在 mysql 命令行中导入
mysql> source /path/to/backup.sql;
```

### 1.4 测试环境的数据脱敏

```sql
-- 从生产库复制到测试库前，对敏感数据脱敏
-- 方式一：mysqldump + sed 替换
-- mysqldump ... | sed 's/real_email@company.com/test@test.com/g' > test_backup.sql

-- 方式二：导入后用 UPDATE 脱敏
UPDATE users SET
    username = CONCAT('test_user_', id),
    email    = CONCAT('test', id, '@test.com'),
    phone    = CONCAT('1380000', LPAD(id, 4, '0'));  -- 13800000001, 13800000002...
```

> 🎯 任何时候从生产库往测试库导数据，**必须先脱敏**！真实用户数据泄露是严重事故。

---

## 二、测试数据工厂 — 批量构造测试数据

### 2.1 用 SQL 批量生成数据

```sql
-- 方法一：利用自增序列（需要一张辅助表）
-- 创建数字辅助表
CREATE TABLE numbers (n INT PRIMARY KEY);
INSERT INTO numbers (n) VALUES
 (1),(2),(3),(4),(5),(6),(7),(8),(9),(10),
 (11),(12),(13),(14),(15),(16),(17),(18),(19),(20);
-- ... 或用存储过程批量生成

-- 方法二：CROSS JOIN 生成笛卡尔积（快速造大量数据）
INSERT INTO users (username, password, email, phone, age, balance)
SELECT
    CONCAT('test_user_', seq.n)       AS username,
    MD5(CONCAT('pass_', seq.n))       AS password,
    CONCAT('test', seq.n, '@test.com') AS email,
    CONCAT('138', LPAD(seq.n, 8, '0')) AS phone,
    18 + (seq.n % 50)                 AS age,
    ROUND(RAND() * 10000, 2)          AS balance
FROM (
    SELECT (a.n*100 + b.n*10 + c.n) AS n
    FROM (SELECT 0 AS n UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4
          UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) a,
         (SELECT 0 AS n UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4
          UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) b,
         (SELECT 0 AS n UNION SELECT 1 UNION SELECT 2 UNION SELECT 3 UNION SELECT 4
          UNION SELECT 5 UNION SELECT 6 UNION SELECT 7 UNION SELECT 8 UNION SELECT 9) c
) seq
WHERE seq.n BETWEEN 1 AND 500;  -- 生成 500 条
-- 这个 SQL 利用了 CROSS JOIN，3 张 10 行表相乘 = 1000，取前 500
```

### 2.2 利用已有多表关系造数据

```sql
-- 给每个用户生成 0~3 个订单（基于已有 users 表）
INSERT INTO orders (user_id, total_amount, status, created_at)
SELECT
    u.id,
    ROUND(RAND() * 5000, 2),
    FLOOR(1 + RAND() * 4),                             -- 随机状态 1~4
    DATE_SUB(NOW(), INTERVAL FLOOR(RAND() * 90) DAY)   -- 过去 90 天内随机
FROM users u
CROSS JOIN (SELECT 1 AS n UNION SELECT 2 UNION SELECT 3) AS t  -- 每个用户最多 3 单
WHERE RAND() < 0.5;  -- 50% 概率生成订单（模拟不是每个人都有订单）

-- 给每个订单生成 1~5 个订单明细
INSERT INTO order_items (order_id, product_id, quantity, price)
SELECT
    o.id,
    FLOOR(1 + RAND() * 3),         -- 随机商品 1~3
    FLOOR(1 + RAND() * 5),         -- 随机数量 1~5
    (SELECT price FROM products WHERE id = FLOOR(1 + RAND() * 3))  -- 商品单价
FROM orders o
CROSS JOIN (SELECT 1 AS n UNION SELECT 2 UNION SELECT 3 UNION SELECT 4 UNION SELECT 5) AS t
WHERE RAND() < 0.4;  -- 每个订单 40% 概率加一项
```

### 2.3 批量数据的边界测试技巧

```sql
-- 构造边界数据：极小值、极大值、NULL、空字符串、特殊字符
INSERT INTO users (username, password, email, balance) VALUES
('edge_min',  'pass', 'min@test.com',    0.00),       -- 余额为 0
('edge_max',  'pass', 'max@test.com',    999999.99),  -- 余额极大值
('edge_null', 'pass', NULL,              0.00),       -- email 为 NULL
('edge_empty','pass', '',                0.00),       -- email 为空字符串
('edge_special','pass','test@test.com',  0.00),       -- 用户名含特殊字符? 试试看
('x',         'pass', 'short@test.com',  0.00);       -- 用户名极短（1 个字符）

-- 构造时间边界
INSERT INTO orders (user_id, total_amount, status, created_at) VALUES
(1, 100, 1, '1970-01-01 00:00:01'),  -- 时间极小值（Unix 纪元后）
(1, 100, 1, '2038-01-19 03:14:07'),  -- 时间极大值（32 位时间戳上限）
(1, 100, 1, DATE_ADD(NOW(), INTERVAL 1 YEAR));  -- 未来时间
```

### 2.4 测试环境的清理与重置

```sql
-- 方法一：TRUNCATE 所有表（重置自增 ID，不可回滚）
SET FOREIGN_KEY_CHECKS = 0;  -- 暂时关闭外键检查
TRUNCATE TABLE order_items;
TRUNCATE TABLE orders;
TRUNCATE TABLE users;
TRUNCATE TABLE products;
SET FOREIGN_KEY_CHECKS = 1;

-- 方法二：DELETE 删除（可回滚，但不重置自增）
START TRANSACTION;
DELETE FROM order_items;
DELETE FROM orders;
DELETE FROM users;
-- 如果确认无误：COMMIT;  如果要恢复：ROLLBACK;

-- 方法三：备份初始状态 → 每次测试后恢复
-- 测试前：mysqldump -u root -p test_study > baseline.sql
-- 测试后：mysql -u root -p test_study < baseline.sql
```

---

## 三、综合实战 — 电商系统全链路数据验证

### 3.1 场景设定

你负责测试一个电商系统。PM 提了以下需求，你需要从数据库层面验证：

- ✅ 用户下单后，订单金额 = 订单明细中各项 `quantity × price` 之和
- ✅ 订单创建后，商品库存相应减少
- ✅ 每人每天的订单数不超过 10 单
- ✅ 所有订单的总金额和订单明细汇总后一致

### 3.2 验证 SQL

```sql
-- 验证一：订单总金额一致性
-- 找出 orders.total_amount ≠ order_items 各项之和的异常订单
SELECT
    o.id,
    o.total_amount,
    SUM(oi.quantity * oi.price) AS 计算金额,
    o.total_amount - SUM(oi.quantity * oi.price) AS 差额
FROM orders o
INNER JOIN order_items oi ON o.id = oi.order_id
GROUP BY o.id
HAVING 差额 != 0;  -- 有任何不一致的都是 Bug！

-- 验证二：库存扣减正确性
-- 简易版（假设每次下单减对应库存，实际系统需记录初始库存）
SELECT
    p.id,
    p.name,
    p.stock AS 当前库存,
    COALESCE(SUM(oi.quantity), 0) AS 已售数量
FROM products p
LEFT JOIN order_items oi ON p.id = oi.product_id
GROUP BY p.id
HAVING 当前库存 < 0;  -- 库存为负 = 超卖！

-- 验证三：单用户单日订单限制
SELECT
    user_id,
    DATE(created_at) AS 日期,
    COUNT(*) AS 当天订单数
FROM orders
GROUP BY user_id, DATE(created_at)
HAVING COUNT(*) > 10;  -- 超过 10 单 = 风控失效！

-- 验证四：全局汇总对账
SELECT
    (SELECT SUM(total_amount) FROM orders) AS 订单总额,
    (SELECT SUM(quantity * price) FROM order_items) AS 明细总额,
    (SELECT SUM(total_amount) FROM orders) - (SELECT SUM(quantity * price) FROM order_items) AS 差异;
-- 差异应为 0
```

### 3.3 测试报告 SQL

```sql
-- 一键生成每日数据质量报告
SELECT
    '订单数量' AS 指标, COUNT(*) AS 值 FROM orders
UNION ALL
SELECT '用户数量', COUNT(*) FROM users
UNION ALL
SELECT '商品数量', COUNT(*) FROM products
UNION ALL
SELECT '今日新增订单', COUNT(*) FROM orders WHERE DATE(created_at) = CURDATE()
UNION ALL
SELECT '待支付订单', COUNT(*) FROM orders WHERE status = 1
UNION ALL
SELECT '已支付订单', COUNT(*) FROM orders WHERE status = 2
UNION ALL
SELECT '已发货订单', COUNT(*) FROM orders WHERE status = 3
UNION ALL
SELECT '已完成订单', COUNT(*) FROM orders WHERE status = 4
UNION ALL
SELECT '异常订单（金额不一致）', COUNT(*) FROM (
    SELECT o.id FROM orders o
    INNER JOIN order_items oi ON o.id = oi.order_id
    GROUP BY o.id
    HAVING o.total_amount != SUM(oi.quantity * oi.price)
) t
UNION ALL
SELECT '库存为负的商品', COUNT(*) FROM products WHERE stock < 0;
```

---

## 四、MySQL 模块知识地图（六天总回顾）

### 4.1 技能树

```
MySQL 测试工程师技能树
│
├── 第一天：数据库基础
│   ├── 关系型数据库概念（表/字段/记录/主键/外键）
│   ├── 数据类型选择（INT/DECIMAL/VARCHAR/CHAR/DATETIME/TEXT）
│   ├── DDL（CREATE TABLE / ALTER TABLE / DROP TABLE）
│   └── 六种约束（PK / FK / NOT NULL / UNIQUE / DEFAULT / CHECK）
│
├── 第二天：数据操作
│   ├── INSERT（单行/批量/部分字段）
│   ├── SELECT（WHERE 13种运算符 / ORDER BY / LIMIT）
│   ├── UPDATE（安全铁律：先 WHERE 后 SET）
│   ├── DELETE vs TRUNCATE vs DROP
│   └── SELECT 子句执行顺序
│
├── 第三天：数据分析
│   ├── 五大聚合函数（COUNT/SUM/AVG/MAX/MIN）
│   ├── GROUP BY（分组统计）
│   ├── HAVING vs WHERE（分组前后过滤）
│   ├── 完整执行顺序（FWGHSOL）
│   └── 字符串函数（CONCAT/SUBSTRING/REPLACE/TRIM）
│
├── 第四天：多表查询
│   ├── INNER JOIN（取交集）
│   ├── LEFT JOIN（保留左表全部 + IS NULL 找差集）
│   ├── RIGHT JOIN（保留右表全部）
│   ├── ON vs WHERE 陷阱（LEFT JOIN 中不同！）
│   └── UNION / UNION ALL（纵向合并）
│
├── 第五天：性能优化
│   ├── 子查询（WHERE/FROM/SELECT 三个位置）
│   ├── IN vs EXISTS
│   ├── B+ 树索引原理
│   ├── 聚簇索引 vs 非聚簇索引（回表）
│   ├── 最左前缀原则（复合索引）
│   ├── 8 种索引失效场景
│   └── EXPLAIN 执行计划
│
├── 第六天：事务安全
│   ├── ACID 四特性
│   ├── 脏读 / 不可重复读 / 幻读
│   ├── 四种隔离级别
│   ├── MVCC 快照读
│   └── 锁机制（S锁/X锁、死锁）
│
└── 第七天（今天）：工程实践
    ├── mysqldump 备份恢复
    ├── 批量测试数据构造
    ├── 边界数据测试
    ├── 环境清理
    └── 全链路数据对账
```

### 4.2 面试高频考点速查表

| 排名 | 考点 | 出现天数 | 一句话答案 |
|:--:|------|:--:|------|
| ⭐⭐⭐ | ACID 四特性 | Day 6 | 原子性（全部成功或全部回滚）、一致性（数据合法）、隔离性（并发互不干扰）、持久性（提交不丢） |
| ⭐⭐⭐ | 四种隔离级别 | Day 6 | RU(未提交)→RC(已提交)→RR(可重复，MySQL默认)→Serializable(串行) |
| ⭐⭐⭐ | 脏读/不可重复读/幻读 | Day 6 | 脏读=未提交、不可重复读=已提交UPDATE、幻读=已提交INSERT |
| ⭐⭐⭐ | 聚簇 vs 非聚簇索引 | Day 5 | 聚簇叶子存整行数据（只有1个），非聚簇存主键值（需回表） |
| ⭐⭐⭐ | 最左前缀原则 | Day 5 | 复合索引(a,b,c)必须从a开始，跳首列则索引失效 |
| ⭐⭐ | WHERE vs HAVING | Day 3 | WHERE分组前过滤（快），HAVING分组后过滤（能用聚合函数） |
| ⭐⭐ | DELETE vs TRUNCATE vs DROP | Day 2 | DELETE删数据可回滚、TRUNCATE清表重置自增、DROP删整表 |
| ⭐⭐ | COUNT(*) vs COUNT(字段) | Day 3 | COUNT(*)不忽略NULL，COUNT(字段)忽略NULL |
| ⭐⭐ | LEFT JOIN ON vs WHERE | Day 4 | ON决定是否匹配，WHERE对右表过滤会让LEFT退化 |
| ⭐⭐ | IN vs EXISTS | Day 5 | IN先子查询再比较，EXISTS逐行判断；子查询小用IN，主表小用EXISTS |
| ⭐ | 索引失效场景 | Day 5 | LIKE前导模糊、对列用函数、类型转换、OR、NOT、复合索引跳首列 |
| ⭐ | EXPLAIN type 排序 | Day 5 | ALL→index→range→ref→eq_ref→const（从左到右越来越好） |

---

## 📝 今日练习

### 练习一：备份恢复

写出以下场景的命令：

| 场景 | 命令 |
|------|------|
| 导出 test_study 整个数据库 | |
| 只导出 users 表 | |
| 导出表结构但不导出数据 | |
| 将备份文件恢复到一个新数据库 | |

### 练习二：批量造数

写一条 SQL，生成 100 条测试商品数据（products 表），要求：
- 商品名格式：`测试商品_001` ~ `测试商品_100`
- 价格：10.00 ~ 999.99 之间随机
- 库存：0 ~ 500 之间随机

### 练习三：综合对账

基于七天来学的知识，设计一套"电商数据对账"的验证 SQL 思路：
1. 你需要验证哪些数据一致性？
2. 每条验证用什么 SQL？

### 练习四：知识地图填空

不看文档，默写 MySQL 七天的学习主题：
- 第一天：___
- 第二天：___
- 第三天：___
- 第四天：___
- 第五天：___
- 第六天：___
- 第七天：___

---

## 📋 自检清单

- [ ] 会用 `mysqldump` 导出整个数据库、单表、只要结构、只要数据吗？
- [ ] 会用 `mysql < backup.sql` 或 `source` 命令恢复数据库吗？
- [ ] 能用 CROSS JOIN 技巧批量生成 500+ 条测试数据吗？
- [ ] 知道怎么构造边界数据（NULL、空串、极值、特殊字符、未来时间）吗？
- [ ] 知道怎么清理测试环境（TRUNCATE vs DELETE + 外键约束处理）吗？
- [ ] 能写出"订单金额 vs 明细汇总"的对账 SQL 吗？
- [ ] 能画出 MySQL 六天的知识地图吗？
- [ ] 面试高频 12 题能脱口而出了吗？

---

> 🎯 **今日小结**：第七天为 MySQL 模块画上句号。今天学的是工程实践——备份恢复保证数据安全，批量造数提高测试效率，数据对账验证业务正确性。这些技能让你从"会写 SQL"升级到"能用数据库完成测试任务"。
>
> 回头看这七天：DDL 建表 → DML 操作 → 聚合分析 → JOIN 联表 → 索引优化 → 事务安全 → 工程实践。你已经具备了测试工程师所需的全部数据库核心能力。**数据库不再是黑盒，而是你的测试利器。**
>
> ---
>
> ## 🐍 下一模块预告：Python 基础
>
> 为什么要学 Python？
>
> ```
> 之前：手动打开 Navicat → 敲 SQL → 看结果 → 人工判断对错
> 之后：Python 脚本 → 自动连数据库 → 执行 SQL → 自动断言 → 生成报告
> ```
>
> **Python 是软件测试自动化的核心语言。** 在接下来的模块中，你将学会：
> - 用 `pymysql` 连接数据库，自动执行 SQL 并验证结果
> - 用 `requests` 发送 HTTP 请求，测试 API 接口
> - 用 `pytest` 编写参数化测试用例，一键执行回归测试
> - 用 `faker` 批量生成逼真的测试数据
>
> > 🚀 准备好了就告诉我，我们开始 **Python 第一天**！
