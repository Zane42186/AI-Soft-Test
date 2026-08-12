# 第一天：Python 环境搭建与基础语法

> 📅 学习日期：第一天 | 🎯 模块：Python 基础 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：为什么测试工程师必须学 Python？

Linux 和 MySQL 模块结束后，你已经具备了"在服务器上查日志"和"在数据库里查数据"的能力。但还有一件事是手工做不来的——**自动化**。

| 场景 | 手工做法 | Python 自动化 |
|------|----------|--------------|
| 🔄 **批量造测试数据** | 一条一条 SQL 手写 | `for` 循环生成 1000 条，一秒搞定 |
| 🌐 **接口测试** | Postman 逐个点 Send | `requests` 库发请求 + 断言响应 |
| 📊 **日志分析** | grep→awk→手动算 | Python 读文件 → 统计 → 生成图表 |
| 🧪 **回归测试** | 手动点 100 个用例 | `pytest` 一键跑完，生成报告 |
| 🔗 **CI/CD 集成** | 人工触发 | GitHub Actions 定时自动跑 |

> 🎯 **一句话**：Python 是软件测试自动化的事实标准语言。面试必问，工作必用。

---

## 一、Python 是什么？

### 1.1 一句话定义

**Python** 是一门**解释型、动态类型、面向对象**的高级编程语言。

和 C++ 的核心差异：

| 维度 | C++ | Python |
|------|-----|--------|
| **编译 vs 解释** | 先编译成机器码，再执行 | 边解释边执行（实际上也有编译为字节码的过程） |
| **类型系统** | 静态类型，变量声明时确定 | 动态类型，运行时确定（鸭子类型） |
| **内存管理** | 手动 `new`/`delete` | 自动垃圾回收（引用计数 + GC） |
| **语法风格** | 花括号 `{}` 定义代码块 | **缩进** 定义代码块 |
| **执行速度** | 快（编译为机器码） | 慢（解释执行），但开发效率高 |
| **最适合** | 系统编程、游戏引擎、高性能计算 | 脚本、自动化、数据分析、AI |

> 📌 你不需要成为 Python 专家。作为测试工程师，你的目标是：**能用 Python 写出自动化脚本 + 看懂别人的代码**。

---

## 二、Python 环境搭建

### 2.1 安装 Python

```bash
# 检查是否已安装
python --version      # Windows
python3 --version     # macOS/Linux

# 如果未安装：
# Windows: https://www.python.org/downloads/ （勾选 "Add Python to PATH"）
# macOS: brew install python3
# Ubuntu: sudo apt install python3
```

### 2.2 虚拟环境（⭐ 必须掌握）

虚拟环境让你为每个项目创建**独立的 Python 环境**，避免依赖冲突。

```bash
# 创建虚拟环境
python -m venv venv            # 在项目目录下创建 venv 文件夹

# 激活虚拟环境
venv\Scripts\activate          # Windows (CMD)
venv\Scripts\Activate.ps1      # Windows (PowerShell)
source venv/bin/activate       # macOS/Linux

# 激活成功后，命令行前面会出现 (venv) 标识
# (venv) C:\project>

# 退出虚拟环境
deactivate
```

> 💡 面试中可能会问 "venv 是什么"，答案："Python 内置的虚拟环境工具，隔离项目依赖。"

### 2.3 pip — 包管理器

```bash
pip install requests           # 安装包
pip install pytest==7.4.0      # 安装指定版本
pip list                       # 查看已安装的包
pip freeze > requirements.txt  # 导出依赖列表
pip install -r requirements.txt # 从文件安装所有依赖
```

> 📌 `pip` 之于 Python，就像 `apt` 之于 Ubuntu、`npm` 之于 Node.js。

---

## 三、Python 基础语法（与 C++ 的对比视角）

### 3.1 第一个 Python 程序

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
"""
第一个 Python 脚本
功能：打印问候语
"""

def greet(name):
    """向指定用户打招呼"""
    return f"Hello, {name}! 欢迎学习 Python 测试自动化。"

# 主程序入口
if __name__ == "__main__":
    print(greet("Zane"))
```

和 C++ 的直观对比：

```cpp
// C++ 版本（对比）
#include <iostream>
#include <string>
using namespace std;

string greet(string name) {
    return "Hello, " + name + "!";
}

int main() {
    cout << greet("Zane") << endl;
    return 0;
}
```

| 差异点 | C++ | Python |
|--------|-----|--------|
| 入口函数 | `int main()` | `if __name__ == "__main__"` |
| 字符串拼接 | `+` 运算符 | `+` 或 f-string `f"{var}"` |
| 行结尾 | 分号 `;` | 不需要分号 |
| 代码块 | `{ }` | **缩进（4 个空格）** |
| 类型声明 | 必须声明 | 不需要 |
| 包含库 | `#include` | `import` |

### 3.2 变量与数据类型

```python
# Python 不需要声明类型，变量只是"标签"
name = "Zane"           # 字符串
age = 25                # 整数
balance = 100.50        # 浮点数
is_vip = True           # 布尔值（注意大写 True/False）
nothing = None          # 空值（类似 C++ 的 nullptr）

# 查看变量类型
print(type(name))       # <class 'str'>
print(type(age))        # <class 'int'>

# 动态类型：变量可以随时指向不同类型
x = 10
x = "hello"             # 合法！但不推荐
```

> 🔑 Python 中一切都是对象。`10` 是 `int` 类的实例，`"hello"` 是 `str` 类的实例。

### 3.3 注释

```python
# 这是单行注释

"""
这是多行注释（文档字符串）
用于函数、类、模块的说明
测试框架会读取这些内容生成报告
"""

def add(a, b):
    """返回两数之和（这是 docstring，可以被 help() 查看）"""
    return a + b
```

### 3.4 运算符

大部分和 C++ 相同，区别如下：

```python
# 算术运算
print(7 / 2)        # 3.5 （普通除法，始终返回 float）
print(7 // 2)       # 3   （整除，向下取整，C++ 的 int 除法默认行为）
print(7 % 2)        # 1   （取模，相同）
print(2 ** 10)      # 1024（幂运算，C++ 用 pow()）

# 比较运算（相同）
print(5 == 5)       # True
print(5 != 3)       # True
print(5 > 3)        # True

# 逻辑运算（用英文单词，不是 && || !）
print(True and False)   # False
print(True or False)    # True
print(not True)         # False

# 成员运算（Python 独有）
nums = [1, 2, 3]
print(2 in nums)        # True   （C++ 需要循环找）
print(5 not in nums)    # True

# 身份运算
a = [1, 2]
b = [1, 2]
c = a
print(a == b)           # True  （值相等）
print(a is b)           # False （不是同一个对象）
print(a is c)           # True  （同一个对象）
```

### 3.5 字符串操作

```python
# 定义字符串（单引号和双引号等价）
s1 = 'hello'
s2 = "world"

# f-string（Python 3.6+，⭐ 最推荐）
name = "Zane"
age = 25
print(f"我叫{name}，今年{age}岁")     # 我叫Zane，今年25岁

# 传统格式化
print("我叫%s，今年%d岁" % (name, age))  # C 风格
print("我叫{}，今年{}岁".format(name, age))  # format 方法

# 常用方法
s = "  Hello, World!  "
print(s.strip())          # "Hello, World!"  去除首尾空格
print(s.lower())          # "  hello, world!  "
print(s.upper())          # "  HELLO, WORLD!  "
print(s.replace("World", "Python"))  # 替换
print(s.split(","))       # ['  Hello', ' World!  ']  分割
print(",".join(["a", "b", "c"]))  # "a,b,c" 拼接

# 切片（⭐ Python 独有）
s = "Hello, World!"
print(s[0:5])     # "Hello"   前 5 个字符
print(s[7:])      # "World!"  从第 7 个到末尾
print(s[::-1])    # "!dlroW ,olleH"  反转字符串
```

> 🎯 **面试常见**："如何反转一个字符串？" → `s[::-1]`

### 3.6 流程控制

#### if-elif-else

```python
score = 85

if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "D"

print(f"成绩等级: {grade}")

# 三元表达式（Python 风格）
result = "通过" if score >= 60 else "不通过"
```

> ⚠️ **Python 没有 `switch-case`**（Python 3.10 加了 `match-case`，但还不常用）。多分支用 `if-elif-else` 或字典映射。

#### for 循环

```python
# 遍历列表
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)

# 遍历字典
user = {"name": "Zane", "age": 25}
for key, value in user.items():
    print(f"{key}: {value}")

# range() 生成数字序列
for i in range(5):           # 0, 1, 2, 3, 4
    print(i)

for i in range(1, 10, 2):    # 1, 3, 5, 7, 9（起始, 结束, 步长）
    print(i)

# enumerate：同时获取索引和值
for index, fruit in enumerate(fruits):
    print(f"{index}: {fruit}")
```

> 📌 Python 的 `for` 更像是 C++ 的 `for (auto& x : container)` 范围 for，不是 `for (int i=0; i<n; i++)`。

#### while 循环

```python
count = 0
while count < 5:
    print(f"计数: {count}")
    count += 1          # 注意：Python 没有 count++

# 死循环 + break
while True:
    user_input = input("输入 q 退出: ")
    if user_input == "q":
        break
```

---

## 四、输入输出

### 4.1 print() 函数

```python
print("Hello")                           # 自动换行
print("Hello", end="")                   # 不换行
print("Name:", name, "Age:", age)        # 逗号自动加空格
print(f"Name: {name}, Age: {age}")       # f-string 推荐
```

### 4.2 input() 函数

```python
name = input("请输入你的名字: ")           # 提示 + 等待输入
print(f"你好, {name}!")

age = int(input("请输入年龄: "))          # input 返回字符串，需手动转类型
```

> ⚠️ `input()` 返回的**始终是字符串**。做数字计算前必须用 `int()` 或 `float()` 转换。

---

## 📝 今日练习

### 练习一：概念对比（10 分钟）

用自己的话解释以下 Python 和 C++ 的核心差异：

| 差异点 | C++ | Python |
|--------|-----|--------|
| 变量声明 | | |
| 代码块定义 | | |
| 编译/解释 | | |
| 内存管理 | | |

### 练习二：写一个年龄判断程序（10 分钟）

编写一个 Python 程序：
1. 提示用户输入年龄
2. 如果年龄 < 18，输出"未成年"
3. 如果 18 ≤ 年龄 < 60，输出"成年"
4. 如果年龄 ≥ 60，输出"老年"
5. 如果输入的不是数字，输出"请输入有效的年龄"

### 练习三：FizzBuzz 面试题（15 分钟）

> ⚠️ 这是测试开发面试中出现的编程题

打印 1~100 的数字，但是：
- 如果数字能被 3 整除，打印 "Fizz"
- 如果数字能被 5 整除，打印 "Buzz"
- 如果数字能同时被 3 和 5 整除，打印 "FizzBuzz"
- 否则打印数字本身

---

## 📋 第一天自检清单

完成学习后，确认以下知识点：

- [ ] Python 环境安装并激活虚拟环境成功了吗？`python --version` 能正常输出吗？
- [ ] 能说出 Python 和 C++ 的 5 个核心差异
- [ ] `print()` 和 `input()` 会用了吗？
- [ ] f-string `f"{变量}"` 的语法掌握了吗？
- [ ] Python 的 `for` 循环和 C++ 有什么不同？
- [ ] `range(5)`、`range(1, 10, 2)` 各生成什么序列？
- [ ] Python 中的逻辑运算符是 `&&` `||` `!` 还是 `and` `or` `not`？
- [ ] `in` 运算符的用途是什么？
- [ ] Python 的 `if-elif-else` 和 `switch-case` 的关系？
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第二天：Python 数据结构深入 — list / tuple / dict / set**

第一天复习了基础语法（你有 C++ 底子，所以过得快），第二天将进入 Python 最核心的数据结构——列表推导式、字典推导式、生成器表达式。这些"Pythonic"的写法也是面试中常被问到的："写一段代码，把列表中的偶数筛选出来"——用列表推导式一行搞定。

---

> ✨ **第一天的核心心法**：Python 是"写起来像伪代码"的语言。你不需要记住所有语法，但一定要在终端里敲一遍。肌肉记忆比大脑记忆更可靠。


# 第二天：Python 数据结构深入 — list / tuple / dict / set

> 📅 学习日期：第二天 | 🎯 模块：Python 基础 → 数据结构 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：数据结构是面试的"必考题"

你有 C++ 基础，STL 的 `vector`、`map`、`set` 你肯定用过。Python 把这些数据结构做得更简洁、更强大。

> 🎯 测试开发面试中，数据结构操作（尤其是列表推导式和字典操作）是**手写代码题的高频考点**。

---

## 一、List（列表）— 最常用的数据结构

### 1.1 基本操作

```python
# 创建列表
fruits = ["apple", "banana", "cherry"]
nums = [1, 2, 3, 4, 5]
mixed = [1, "hello", 3.14, True]  # 可以混合类型（C++ vector 不行）

# 访问元素（索引、切片）
print(fruits[0])        # "apple" 第一个
print(fruits[-1])       # "cherry" 倒数第一个
print(fruits[0:2])      # ['apple', 'banana'] 前 2 个

# 修改
fruits[1] = "orange"    # 直接赋值
fruits.append("grape")  # 尾部追加（C++ push_back）
fruits.insert(1, "kiwi") # 指定位置插入
fruits.remove("apple")  # 按值删除（删第一个匹配的）
popped = fruits.pop()   # 弹出尾部（C++ pop_back，但 Python 返回被删的值）
popped = fruits.pop(0)  # 弹出指定位置
```

### 1.2 列表推导式（List Comprehension）⭐

> ⚠️ **面试高频考点，必须掌握！**

```python
# 传统写法：筛选偶数
nums = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
evens = []
for n in nums:
    if n % 2 == 0:
        evens.append(n)
print(evens)  # [2, 4, 6, 8, 10]

# 列表推导式：一行搞定！
evens = [n for n in nums if n % 2 == 0]

# 语法：[表达式 for 变量 in 可迭代对象 if 条件]
squares = [x**2 for x in range(10)]           # [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
uppers = [s.upper() for s in fruits]           # 全部转大写
pairs = [(x, y) for x in range(3) for y in range(3)]  # 嵌套循环→笛卡尔积
```

> 🎯 **面试常见**："把下面这个 for 循环改成列表推导式"——考官在考你 Python 熟不熟练。

### 1.3 列表常用方法

```python
nums = [3, 1, 4, 1, 5, 9, 2, 6]

nums.sort()                 # 原地排序
sorted_nums = sorted(nums)  # 返回新列表，原列表不变
nums.reverse()              # 原地反转
len(nums)                   # 长度
min(nums), max(nums)        # 最小/最大值
sum(nums)                   # 求和（超级实用！）
nums.count(1)               # 统计 1 出现的次数
nums.index(5)               # 查找 5 的索引位置

# 列表拼接
combined = [1, 2] + [3, 4]  # [1, 2, 3, 4]
repeated = [0] * 5           # [0, 0, 0, 0, 0]
```

### 1.4 测试场景：用列表推导式生成测试数据

```python
# 生成 100 个测试用户名
users = [f"test_user_{i:03d}" for i in range(1, 101)]
# ['test_user_001', 'test_user_002', ..., 'test_user_100']

# 生成 50 个随机手机号（1 开头）
import random
phones = [f"1{random.randint(1000000000, 9999999999)}" for _ in range(50)]
# 注意：_ 作为变量名表示"我不关心这个值"

# 筛选出包含 "error" 的日志行（大小写不敏感）
logs = ["INFO: ok", "ERROR: timeout", "error: crash", "DEBUG: done"]
errors = [line for line in logs if "error" in line.lower()]
# ['ERROR: timeout', 'error: crash']
```

---

## 二、Tuple（元组）— 不可变的列表

### 2.1 基本操作

```python
# 创建元组
point = (3, 5)
person = ("Zane", 25, "北京")
single = (1,)           # 单元素元组必须加逗号！（否则 (1) 就是数字 1）

# 元组不可变
# point[0] = 10         # ❌ 报错！TypeError

# 元组解包（⭐ Python 独有）
x, y = point            # x=3, y=5
name, age, city = person

# 一行交换两个变量的值（面试常考）
a, b = 1, 2
a, b = b, a             # a=2, b=1  无需临时变量！

# 函数返回多个值（本质是返回元组）
def get_position():
    return 100, 200     # 返回元组 (100, 200)

x, y = get_position()
```

### 2.2 Tuple vs List — 什么时候用哪个？

| 维度 | List | Tuple |
|------|------|-------|
| **可变性** | 可增删改 | 创建后不可变 |
| **性能** | 内存更大，操作稍慢 | 内存更小，操作更快 |
| **哈希** | 不可哈希（不能做 dict 的 key） | 可哈希（能做 dict 的 key） |
| **语义** | "一组同类的可变数据" | "一个固定的记录"（如坐标、数据库行） |

> 🎯 **面试回答**："Tuple 不可变、可哈希、更快。适合做函数多返回值、字典 key、以及那些你不希望被修改的数据。"

---

## 三、Dict（字典）— 最强大的数据结构

### 3.1 基本操作

```python
# 创建字典
user = {"name": "Zane", "age": 25, "city": "北京"}
empty = {}                              # 空字典
from_pairs = dict([("a", 1), ("b", 2)]) # 从键值对列表创建

# 访问
print(user["name"])          # "Zane"      key 不存在会报错
print(user.get("email"))     # None        key 不存在返回 None（安全）
print(user.get("email", "未设置"))  # "未设置"   可指定默认值

# 修改
user["age"] = 26             # 修改已有 key
user["email"] = "zane@test.com"  # 新增 key-value

# 删除
del user["city"]             # 删除指定 key
email = user.pop("email")    # 弹出并返回值

# 遍历
for key in user:                          # 遍历 key
for key, value in user.items():           # 遍历 key-value（⭐ 最常用）
for value in user.values():               # 遍历 value
```

### 3.2 字典推导式

```python
# 键值互换
original = {"a": 1, "b": 2, "c": 3}
swapped = {v: k for k, v in original.items()}
# {1: 'a', 2: 'b', 3: 'c'}

# 筛选
scores = {"Alice": 85, "Bob": 72, "Charlie": 90, "David": 60}
passed = {name: score for name, score in scores.items() if score >= 80}
# {'Alice': 85, 'Charlie': 90}

# 平方表
squares = {x: x**2 for x in range(5)}
# {0: 0, 1: 1, 2: 4, 3: 9, 4: 16}
```

### 3.3 嵌套结构（测试数据常用）

```python
# 列表套字典 — 最像数据库查询结果的格式
users = [
    {"id": 1, "name": "张三", "age": 25, "balance": 100.00},
    {"id": 2, "name": "李四", "age": 30, "balance": 200.00},
    {"id": 3, "name": "王五", "age": 22, "balance": 50.00},
]

# 筛选余额 > 80 的用户
rich_users = [u for u in users if u["balance"] > 80]

# 字典套列表
test_data = {
    "urls": ["/api/login", "/api/register", "/api/user/info"],
    "users": ["admin", "test001", "test002"],
    "passwords": ["Admin@123", "Test@123", "Test@456"],
}
```

### 3.4 测试场景：用 Dict 做测试用例数据驱动

```python
# 一个测试用例就是一个 dict
test_case = {
    "case_id": "TC-LOGIN-001",
    "title": "正确用户名+正确密码登录",
    "url": "/api/login",
    "method": "POST",
    "data": {"username": "admin", "password": "Pass1234"},
    "expected_status": 200,
    "expected_body": {"code": 0, "msg": "登录成功"},
}
```

---

## 四、Set（集合）— 去重 + 集合运算

### 4.1 基本操作

```python
# 创建集合
fruits = {"apple", "banana", "cherry"}
empty = set()               # 空集合（{} 创建的是空字典！）
from_list = set([1, 2, 2, 3, 3, 3])  # {1, 2, 3} 自动去重

# 增删
fruits.add("grape")
fruits.remove("apple")      # 元素不存在会报错
fruits.discard("mango")     # 元素不存在也不报错（安全删除）

# 集合运算（⭐ 非常实用）
a = {1, 2, 3, 4}
b = {3, 4, 5, 6}

print(a | b)    # 并集 {1, 2, 3, 4, 5, 6}
print(a & b)    # 交集 {3, 4}
print(a - b)    # 差集 {1, 2}（a 有而 b 没有的）
print(a ^ b)    # 对称差 {1, 2, 5, 6}（只在一边出现的）
```

### 4.2 测试场景

```python
# 场景 1：列表去重（面试常见）
nums = [1, 2, 2, 3, 3, 3, 4, 4, 4, 4]
unique = list(set(nums))    # [1, 2, 3, 4]

# 场景 2：找出两个接口返回结果的差异
api_v1_users = {"张三", "李四", "王五", "赵六"}
api_v2_users = {"张三", "李四", "王五", "田七"}

new_users = api_v2_users - api_v1_users       # {'田七'}  新增用户
removed_users = api_v1_users - api_v2_users    # {'赵六'}  被删除的用户
common_users = api_v1_users & api_v2_users     # {'张三', '李四', '王五'} 不变用户

# 场景 3：快速判断元素是否存在（O(1) 时间复杂度）
blacklist = {"user123", "user456", "user789"}
if "user999" not in blacklist:
    print("允许操作")
```

---

## 五、四种数据结构对比总结

| 维度 | List | Tuple | Dict | Set |
|------|------|-------|------|-----|
| **可变性** | ✅ 可变 | ❌ 不可变 | ✅ 可变 | ✅ 可变 |
| **有序性** | ✅ 有序 | ✅ 有序 | ✅ 有序（Python 3.7+） | ❌ 无序 |
| **重复元素** | ✅ 允许 | ✅ 允许 | key 唯一，value 可重复 | ❌ 不允许 |
| **查找速度** | O(n) | O(n) | O(1)（按 key） | O(1) |
| **类比 C++** | `std::vector` | `std::array` / `const vector` | `std::unordered_map` | `std::unordered_set` |
| **面试高频** | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |
| **最常用在** | 批量数据、遍历 | 函数返回值、坐标 | 配置、JSON 数据 | 去重、集合运算 |

---

## 📝 今日练习

### 练习一：列表推导式（10 分钟）

用**一行列表推导式**完成以下需求：

| 需求 | 你的答案 |
|------|----------|
| 1. 生成 1~20 的所有偶数列表 | |
| 2. 把 `["hello", "world", "python"]` 全部转大写 | |
| 3. 筛选出 `[85, 92, 78, 65, 95, 88]` 中 ≥ 80 的分数 | |
| 4. 生成 `[(1,1), (1,2), (1,3), (2,1), (2,2), (2,3)]` | |

### 练习二：Dict 操作（15 分钟）

```python
orders = [
    {"id": 1, "user": "张三", "amount": 99.00, "status": "已支付"},
    {"id": 2, "user": "李四", "amount": 299.00, "status": "待支付"},
    {"id": 3, "user": "张三", "amount": 199.00, "status": "已支付"},
    {"id": 4, "user": "王五", "amount": 499.00, "status": "已取消"},
    {"id": 5, "user": "李四", "amount": 149.00, "status": "已支付"},
]
```

写出完成以下需求的 Python 代码：

| 需求 | 你的答案 |
|------|----------|
| 1. 统计"已支付"状态的订单数量 | |
| 2. 计算"已支付"订单的总金额 | |
| 3. 找出每个用户的下单次数（结果如 `{"张三": 2, "李四": 2, "王五": 1}`） | |
| 4. 找出下单金额最高的那条订单信息（整个 dict） | |

### 练习三：Set 实战（10 分钟）

有两个测试结果集，找出差异：

```python
test_result_v1 = {"TC001", "TC002", "TC003", "TC004", "TC005"}
test_result_v2 = {"TC002", "TC003", "TC004", "TC006", "TC007"}
```

| 需求 | 你的答案 |
|------|----------|
| 1. v2 中新增了哪些用例？ | |
| 2. v1 中有但 v2 中删除了哪些用例？ | |
| 3. 两个版本都通过的用例有哪些？ | |

---

## 📋 第二天自检清单

- [ ] 能用列表推导式一行完成筛选 + 转换
- [ ] 知道 `tuple` 和 `list` 的区别（可变性、性能、哈希）
- [ ] 能用 `a, b = b, a` 一行交换变量值
- [ ] dict 的 `get(key, default)` 和直接 `[key]` 的区别是什么？
- [ ] 能写出字典推导式做键值互换
- [ ] `set` 的 4 种集合运算（并/交/差/对称差）
- [ ] 知道如何用 `set` 做列表去重
- [ ] 能说出四种数据结构的 C++ 对应物
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第三天：函数进阶 — lambda、生成器、装饰器入门**

掌握了数据结构后，第三天将进入 Python 函数的高级特性——lambda 匿名函数、生成器（yield）、函数参数的各种玩法（`*args`/`**kwargs`），以及装饰器的基本概念。这些都是让你写出"Pythonic"代码的关键。

---

> ✨ **第二天的核心心法**：数据结构的熟练程度 = Python 水平的一半。把列表推导式和字典操作练到"不需要查资料就能写出来"，你就超越了 60% 的 Python 初学者。


# 第三天：函数进阶 — lambda、生成器、装饰器入门

> 📅 学习日期：第三天 | 🎯 模块：Python 基础 → 函数进阶 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：从"能写函数"到"写好函数"

前两天我们复习了基础语法和数据结构。今天进入 Python 函数的进阶特性——这些是 C++ 中没有或差异很大的概念，也是 Python 代码"简洁优雅"的核心来源。

> 🎯 面试中，lambda 和列表推导式经常组合出现；生成器的内存优化原理是进阶面试题；装饰器是框架（如 pytest）的底层机制。

---

## 一、函数参数的各种玩法

### 1.1 参数类型回顾

```python
# 位置参数（最普通）
def greet(name, age):
    print(f"{name}, {age}")

greet("Zane", 25)               # 按位置传递

# 关键字参数（可以不按顺序）
greet(age=25, name="Zane")      # 效果相同

# 默认参数（有默认值的参数放后面）
def greet(name, age=18, city="北京"):
    print(f"{name}, {age}岁, 来自{city}")

greet("Zane")                        # Zane, 18岁, 来自北京
greet("Zane", 25, "上海")            # Zane, 25岁, 来自上海
```

> ⚠️ **默认参数的坑**：默认值不要用可变对象（如 list、dict）！
> ```python
> # ❌ 错误：默认参数只在函数定义时计算一次
> def add_item(item, items=[]):
>     items.append(item)
>     return items
>
> print(add_item(1))  # [1]
> print(add_item(2))  # [1, 2] ← 预期是 [2]！
>
> # ✅ 正确
> def add_item(item, items=None):
>     if items is None:
>         items = []
>     items.append(item)
>     return items
> ```

### 1.2 `*args` 和 `**kwargs`（⭐ 重要）

```python
# *args：接收任意数量的位置参数（打包成元组）
def sum_all(*args):
    return sum(args)

print(sum_all(1, 2, 3, 4, 5))       # 15

# **kwargs：接收任意数量的关键字参数（打包成字典）
def print_info(**kwargs):
    for key, value in kwargs.items():
        print(f"{key}: {value}")

print_info(name="Zane", age=25, city="北京")

# 组合使用：固定参数 + *args + **kwargs
def full_function(a, b, *args, **kwargs):
    print(f"a={a}, b={b}")
    print(f"args={args}")
    print(f"kwargs={kwargs}")

full_function(1, 2, 3, 4, 5, name="Zane", age=25)
# a=1, b=2
# args=(3, 4, 5)
# kwargs={'name': 'Zane', 'age': 25}
```

> 🎯 **测试场景**：写测试框架的装饰器或 fixture 时，经常用 `*args` 和 `**kwargs` 来传递不确定数量的参数。

### 1.3 参数解包（反向操作）

```python
# * 解包列表/元组
nums = [1, 2, 3]
print(*nums)            # 等效于 print(1, 2, 3)

# ** 解包字典
user = {"name": "Zane", "age": 25}
greet(**user)           # 等效于 greet(name="Zane", age=25)

# 实战：合并字典（Python 3.5+）
dict1 = {"a": 1, "b": 2}
dict2 = {"c": 3, "d": 4}
merged = {**dict1, **dict2}    # {'a': 1, 'b': 2, 'c': 3, 'd': 4}
```

---

## 二、Lambda 表达式（匿名函数）

### 2.1 基本语法

```python
# 语法：lambda 参数: 返回值
add = lambda x, y: x + y
print(add(3, 5))        # 8

# 等效于
def add(x, y):
    return x + y
```

lambda 的核心优势：**作为参数传给其他函数**。

### 2.2 lambda + sorted / map / filter

```python
users = [
    {"name": "张三", "age": 25},
    {"name": "李四", "age": 30},
    {"name": "王五", "age": 22},
]

# sorted + lambda：按年龄排序
sorted_users = sorted(users, key=lambda u: u["age"])
# [{'name': '王五', 'age': 22}, {'name': '张三', 'age': 25}, {'name': '李四', 'age': 30}]

# 按年龄倒序
sorted_users = sorted(users, key=lambda u: u["age"], reverse=True)

# map + lambda：对每个元素做转换
nums = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, nums))  # [2, 4, 6, 8, 10]

# filter + lambda：筛选
evens = list(filter(lambda x: x % 2 == 0, nums))  # [2, 4]
```

> 📌 列表推导式通常比 `map`/`filter` + lambda 更可读。面试中两者都可能出现，建议**优先用列表推导式**：
> ```python
> doubled = [x * 2 for x in nums]           # 比 map 更 Pythonic
> evens = [x for x in nums if x % 2 == 0]    # 比 filter 更可读
> ```

---

## 三、生成器（Generator）— 内存友好的迭代器

### 3.1 什么是生成器？

> 🎯 **核心价值**：生成器不会一次性把所有数据加载到内存，而是**用多少取多少**。

```python
# 列表：一次性创建 1000 万个元素 → 占大量内存
# nums = [x for x in range(10000000)]  # 危险！可能撑爆内存

# 生成器：需要时再生成 → 几乎不占内存
nums = (x for x in range(10000000))    # 生成器表达式（注意是圆括号）
print(next(nums))  # 0
print(next(nums))  # 1
print(next(nums))  # 2
```

### 3.2 yield — 创建生成器函数

```python
# 普通函数：一次返回全部
def get_numbers_list(n):
    result = []
    for i in range(n):
        result.append(i)
    return result

# 生成器函数：逐个产出
def get_numbers_generator(n):
    for i in range(n):
        yield i    # ← yield 是关键！不会终止函数，只是"暂停"

gen = get_numbers_generator(5)
print(next(gen))  # 0
print(next(gen))  # 1
print(list(gen))  # [2, 3, 4]
```

### 3.3 yield vs return 对比

| | return | yield |
|------|--------|-------|
| **行为** | 终止函数，返回一个值 | 暂停函数，返回一个值，下次从暂停处继续 |
| **返回值** | 任意对象 | 生成器对象（可迭代） |
| **内存** | 一次性返回全部数据 | 逐个产出，省内存 |
| **可迭代次数** | 1 次 | 通常 1 次（用完后需重新创建） |

### 3.4 测试场景：逐行读取大日志文件

```python
# 不用生成器：100MB 的日志一次性读到内存 → 可能会卡死
def read_logs_bad(filepath):
    with open(filepath, "r") as f:
        return f.readlines()        # 整文件读入内存

# 用生成器：逐行产出，内存占用恒定
def read_logs(filepath):
    with open(filepath, "r") as f:
        for line in f:
            if "ERROR" in line:     # 只产出感兴趣的行
                yield line.strip()

# 使用
for error_line in read_logs("/var/log/app.log"):
    print(error_line)
```

---

## 四、装饰器入门（了解即可）

### 4.1 什么是装饰器？

> 🎯 **一句话**：装饰器是一个函数，它接收一个函数作为参数，返回一个新函数——在不修改原函数代码的情况下增加功能。

```python
# 一个最简单的装饰器
def log_call(func):
    def wrapper(*args, **kwargs):
        print(f"调用函数: {func.__name__}")
        result = func(*args, **kwargs)
        print(f"{func.__name__} 执行完成")
        return result
    return wrapper

# 使用装饰器
@log_call
def add(a, b):
    return a + b

print(add(3, 5))
# 输出：
# 调用函数: add
# add 执行完成
# 8
```

### 4.2 测试中的装饰器应用

```python
import time

# 计算函数执行时间
def timer(func):
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        elapsed = time.time() - start
        print(f"{func.__name__} 耗时: {elapsed:.4f} 秒")
        return result
    return wrapper

@timer
def slow_function():
    time.sleep(1.5)

# slow_function()
# 输出：slow_function 耗时: 1.5001 秒
```

> 📌 `pytest` 中的 `@pytest.fixture`、`@pytest.mark.parametrize` 本质上都是装饰器。先了解概念，后面学 pytest 时会看到大量装饰器的实际应用。

---

## 五、常用内置函数速查

```python
# 类型转换
int("123"), str(456), float("3.14"), bool(1)

# 数学计算
abs(-5), round(3.14159, 2), pow(2, 10)

# 序列操作
len([1, 2, 3]), sum([1, 2, 3]), min(1, 2, 3), max(1, 2, 3)
sorted([3, 1, 2]), reversed([1, 2, 3])

# 枚举和打包
list(enumerate(["a", "b", "c"]))     # [(0, 'a'), (1, 'b'), (2, 'c')]
list(zip([1, 2, 3], ["a", "b", "c"]))  # [(1, 'a'), (2, 'b'), (3, 'c')]

# 判断
all([True, True, False])   # False（全部为 True？）
any([False, False, True])  # True （任一为 True？）
isinstance(25, int)        # True （类型检查）

# 帮助
help(str), dir(str), type("hello")
```

---

## 📝 今日练习

### 练习一：lambda 实战（10 分钟）

用一行代码完成：

| 需求 | 你的答案（一行 lambda） |
|------|------------------------|
| 1. 把 `["hello", "world"]` 转大写 | |
| 2. 筛选出 `[15, 60, 45, 80, 30, 90]` 中 > 50 的数 | |
| 3. 按字符串长度排序 `["a", "abc", "ab", "abcd"]` | |
| 4. 按年龄升序排列用户列表（用户 Dict 含 name 和 age） | |

### 练习二：生成器练习（10 分钟）

1. 写一个生成器函数 `fibonacci(n)`，生成前 n 个斐波那契数列
2. 写一个生成器表达式，生成 1~100 中所有能被 7 整除的数

### 练习三：`*args` / `**kwargs` 练习（10 分钟）

1. 写一个函数 `multi_max(*args)`，返回任意数量参数中的最大值（不允许用内置 `max()`）
2. 写一个函数 `filter_kwargs(**kwargs)`，过滤出值大于 10 的键值对，返回新字典

---

## 📋 第三天自检清单

- [ ] 能解释 `*args` 和 `**kwargs` 的区别和用途
- [ ] 知道默认参数为什么不能用可变对象（`[]`、`{}`）
- [ ] lambda 的语法是什么？什么场景下用它最合适？
- [ ] 生成器和列表的区别是什么（内存方面）？
- [ ] `yield` 关键字的作用是什么？
- [ ] 装饰器的本质是什么（用一句话概括）？
- [ ] `zip()` 和 `enumerate()` 分别是做什么的？
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第四天：文件操作与异常处理**

第三天把函数玩明白了，第四天将学习如何用 Python 读写文件（TXT/CSV/JSON）——这是测试中最常见的需求：读取测试数据文件、写入测试报告、处理配置文件。同时学习异常处理（`try/except`），让脚本更健壮。

---

> ✨ **第三天的核心心法**：生成器让你处理无限大的数据，lambda 让你写更简洁的回调，装饰器让你在不改代码的情况下加功能。这三样学好了，代码就能从"能用"进化为"优雅"。


# 第四天：文件操作与异常处理

> 📅 学习日期：第四天 | 🎯 模块：Python 基础 → 文件与异常 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：测试工程师的日常 = 读文件 + 写文件

测试工作中，你几乎每天都要：

- 📄 读取测试数据文件（CSV / JSON / YAML）
- 📝 写入测试报告（HTML / JSON / TXT）
- 📋 处理配置文件（读取环境配置）
- 🔍 分析日志文件（上一模块学的 Linux 命令的 Python 版）

> 🎯 Python 的文件操作比 C++ 简单 10 倍——用 `with` 语句自动管理资源，再也不用手动 `close()`。

---

## 一、文件操作基础

### 1.1 打开文件的模式

```python
# 完整语法
# open(file, mode='r', encoding='utf-8')

# 常用模式
# 'r'  读取（默认）— 文件不存在会报错
# 'w'  写入 — 文件存在则清空，不存在则创建
# 'a'  追加 — 在文件末尾添加，不存在则创建
# 'x'  创建 — 创建新文件，存在则报错
# 'b'  二进制模式（配合 'rb'、'wb'）
# '+'  读写模式（'r+'）

# 推荐始终指定 encoding='utf-8'
```

### 1.2 with 语句（⭐ 最重要的习惯）

```python
# ❌ 不推荐：需要手动 close
f = open("test.txt", "r")
content = f.read()
f.close()           # 容易忘记！

# ✅ 推荐：with 语句自动管理资源（类似 C++ 的 RAII）
with open("test.txt", "r", encoding="utf-8") as f:
    content = f.read()
# 缩进块结束后，文件自动关闭，即使发生异常也会关
```

> 📌 `with` 语句不仅用于文件，数据库连接、网络 socket、锁等资源管理都用它。

### 1.3 读取文件

```python
# 方法一：read() — 一次性读入整个文件（小文件用）
with open("config.json", "r", encoding="utf-8") as f:
    content = f.read()

# 方法二：readlines() — 读入所有行，返回列表
with open("app.log", "r", encoding="utf-8") as f:
    lines = f.readlines()

# 方法三：逐行迭代（⭐ 大文件推荐，内存友好）
with open("app.log", "r", encoding="utf-8") as f:
    for line in f:
        if "ERROR" in line:
            print(line.strip())
```

### 1.4 写入文件

```python
# write() — 写入字符串
with open("output.txt", "w", encoding="utf-8") as f:
    f.write("第一行\n")
    f.write("第二行\n")

# writelines() — 写入字符串列表
lines = ["行1\n", "行2\n", "行3\n"]
with open("output.txt", "w", encoding="utf-8") as f:
    f.writelines(lines)

# 追加模式 — 不清空原文件
with open("app.log", "a", encoding="utf-8") as f:
    f.write(f"{datetime.now()}: 新的日志条目\n")
```

---

## 二、CSV 文件操作

### 2.1 为什么测试工程师需要处理 CSV？

CSV 是测试数据管理中最常用的格式——**Excel 能打开、文本编辑器能看、所有语言都支持**。

### 2.2 使用 csv 模块

```python
import csv

# ===== 读取 CSV =====
with open("test_data.csv", "r", encoding="utf-8") as f:
    reader = csv.DictReader(f)   # 每行变 dict，表头作为 key
    for row in reader:
        print(row["用户名"], row["密码"], row["预期结果"])

# ===== 写入 CSV =====
data = [
    {"用例编号": "TC001", "用户名": "admin", "密码": "123456", "预期": "登录成功"},
    {"用例编号": "TC002", "用户名": "", "密码": "123456", "预期": "用户名为空"},
    {"用例编号": "TC003", "用户名": "admin", "密码": "wrong", "预期": "密码错误"},
]

with open("test_cases.csv", "w", encoding="utf-8-sig", newline="") as f:
    # utf-8-sig 解决 Excel 打开中文乱码问题
    writer = csv.DictWriter(f, fieldnames=["用例编号", "用户名", "密码", "预期"])
    writer.writeheader()    # 写入表头
    writer.writerows(data)  # 批量写入
```

> 💡 `newline=""` 是为了防止 Windows 上出现多余空行。

### 2.3 实战：批量生成测试数据写入 CSV

```python
import csv
import random
import string

def generate_test_users(count=100):
    """生成测试用户数据"""
    users = []
    for i in range(1, count + 1):
        username = f"test_user_{i:04d}"
        password = ''.join(random.choices(string.ascii_letters + string.digits, k=10))
        phone = f"1{random.randint(1000000000, 9999999999)}"
        age = random.randint(18, 65)
        users.append({
            "用户名": username,
            "密码": password,
            "手机号": phone,
            "年龄": age
        })
    return users

# 生成并写入 CSV
users = generate_test_users(100)
with open("test_users.csv", "w", encoding="utf-8-sig", newline="") as f:
    writer = csv.DictWriter(f, fieldnames=["用户名", "密码", "手机号", "年龄"])
    writer.writeheader()
    writer.writerows(users)

print("100 条测试用户数据已生成！")
```

---

## 三、JSON 文件操作

### 3.1 为什么需要 JSON？

JSON 是现代 API 的数据格式标准。测试接口时，请求体和响应体都是 JSON。你的测试数据、配置文件、测试报告也经常是 JSON。

### 3.2 使用 json 模块

```python
import json

# ===== Python dict → JSON 字符串 =====
data = {"name": "Zane", "age": 25, "skills": ["Python", "Linux", "MySQL"]}
json_str = json.dumps(data, ensure_ascii=False, indent=2)
print(json_str)
# 输出：
# {
#   "name": "Zane",
#   "age": 25,
#   "skills": ["Python", "Linux", "MySQL"]
# }

# ===== JSON 字符串 → Python dict =====
json_str = '{"name": "Zane", "age": 25}'
data = json.loads(json_str)
print(data["name"])     # "Zane"

# ===== 读写 JSON 文件 =====
# 写入
with open("config.json", "w", encoding="utf-8") as f:
    json.dump(data, f, ensure_ascii=False, indent=2)

# 读取
with open("config.json", "r", encoding="utf-8") as f:
    config = json.load(f)
```

| 函数 | 操作对象 | 方向 |
|------|----------|------|
| `json.dumps()` | Python dict → JSON **字符串** | 序列化 |
| `json.loads()` | JSON **字符串** → Python dict | 反序列化 |
| `json.dump()` | Python dict → JSON **文件** | 序列化到文件 |
| `json.load()` | JSON **文件** → Python dict | 从文件反序列化 |

> 📌 记忆技巧：带 `s` 的 = string（操作字符串），不带 `s` 的 = file（操作文件）。

### 3.3 测试场景：读取 JSON 配置 + 接口测试数据

```python
import json

# 读取接口测试用例文件
with open("api_test_cases.json", "r", encoding="utf-8") as f:
    test_cases = json.load(f)

# test_cases.json 内容示例：
# [
#   {"method": "POST", "url": "/api/login", "data": {"username": "admin", "password": "Pass123"}, "expected_code": 200},
#   {"method": "POST", "url": "/api/login", "data": {"username": "", "password": "Pass123"}, "expected_code": 400},
#   {"method": "GET",  "url": "/api/user/info", "expected_code": 200}
# ]

for case in test_cases:
    print(f"测试用例: {case['method']} {case['url']}")
    # 之后配合 requests 库执行（Day 6 会学）
```

---

## 四、异常处理

### 4.1 为什么需要异常处理？

测试脚本在运行过程中，可能遇到：
- 文件不存在
- 网络请求超时
- JSON 格式错误
- 数据库连接失败

如果不用异常处理，脚本会直接崩溃，后面的用例就执行不了了。

### 4.2 try/except/else/finally

```python
# 基本结构
try:
    # 可能出错的代码
    result = 10 / 0
except ZeroDivisionError:
    # 捕获指定异常
    print("除数不能为零！")
except Exception as e:
    # 捕获所有异常（万能兜底）
    print(f"未知错误: {e}")
else:
    # try 块没有异常时执行
    print("执行成功！")
finally:
    # 无论是否异常都会执行（清理资源）
    print("清理工作完成")
```

### 4.3 常见内置异常

| 异常类型 | 触发场景 |
|----------|----------|
| `ValueError` | 类型对了但值不对（如 `int("abc")`） |
| `TypeError` | 类型错误（如 `"hello" + 5`） |
| `FileNotFoundError` | 文件不存在 |
| `KeyError` | 字典中找不到 key |
| `IndexError` | 列表索引越界 |
| `ZeroDivisionError` | 除零 |
| `JSONDecodeError` | JSON 解析失败 |
| `ConnectionError` | 网络连接失败 |
| `AssertionError` | assert 断言失败 |

### 4.4 测试场景：健壮的测试脚本

```python
import json
import requests

def execute_test_case(case):
    """执行一条测试用例，返回是否通过"""
    try:
        # 发送请求
        if case["method"] == "POST":
            response = requests.post(case["url"], json=case["data"], timeout=5)
        else:
            response = requests.get(case["url"], timeout=5)
        
        # 断言状态码
        assert response.status_code == case["expected_code"], \
            f"状态码不匹配: 期望 {case['expected_code']}, 实际 {response.status_code}"
        
        return True, None
    except requests.Timeout:
        return False, "请求超时"
    except requests.ConnectionError:
        return False, "连接失败"
    except KeyError as e:
        return False, f"用例缺少字段: {e}"
    except AssertionError as e:
        return False, str(e)
    except Exception as e:
        return False, f"未知错误: {e}"

# 批量执行
test_cases = [
    {"method": "POST", "url": "http://localhost:8080/api/login", "data": {"username": "admin"}, "expected_code": 200},
    # ... 更多用例
]

passed = 0
failed = 0
for case in test_cases:
    success, error = execute_test_case(case)
    if success:
        passed += 1
    else:
        failed += 1
        print(f"❌ 失败: {error}")

print(f"结果: {passed} 通过, {failed} 失败")
```

### 4.5 自定义异常

```python
# 定义测试专用的异常
class TestDataError(Exception):
    """测试数据异常"""
    pass

class AssertionFailedError(Exception):
    """断言失败异常"""
    pass

# 使用
def validate_test_case(case):
    if "url" not in case:
        raise TestDataError(f"用例缺少必需字段 'url': {case}")

try:
    validate_test_case({"method": "POST"})  # 没有 url
except TestDataError as e:
    print(f"数据校验失败: {e}")
```

---

## 📝 今日练习

### 练习一：文件读写（15 分钟）

1. 创建一个文本文件 `test_log.txt`，写入 10 行模拟日志（至少包含 3 行 ERROR 和 2 行 WARNING）
2. 用 Python 读取这个文件，筛选出所有包含 "ERROR" 的行，写入 `error_only.txt`
3. 统计每种日志级别的出现次数（INFO / WARNING / ERROR 各有多少行）

### 练习二：CSV 测试数据生成（15 分钟）

写一个 Python 脚本，生成 50 条登录测试用例并写入 CSV：

- 包含字段：用例编号、用户名、密码、预期结果
- 其中 5 条是正向用例（用户名和密码都正确）
- 45 条是反向用例（覆盖用户名为空、密码为空、用户名不存在、密码错误等情况）

### 练习三：JSON + 异常处理（15 分钟）

1. 创建一个 JSON 文件，包含至少 5 条接口测试用例（method、url、data、expected_code）
2. 写 Python 脚本读取这个 JSON 文件
3. 为 JSON 解析和字段校验添加异常处理
4. 如果某条用例缺少 `url` 字段，跳过后继续执行后面的用例

---

## 📋 第四天自检清单

- [ ] `open()` 的 `'r'` / `'w'` / `'a'` 模式分别是什么作用？
- [ ] `with` 语句相比手动 `open/close` 有什么优势？
- [ ] 大文件读取用什么方式（`read()` vs 逐行迭代）？
- [ ] `csv.DictReader` 和 `csv.DictWriter` 的用法
- [ ] `json.dumps()` / `json.loads()` / `json.dump()` / `json.load()` 四者的区别
- [ ] `ensure_ascii=False` 的作用是什么？
- [ ] `try/except/else/finally` 的执行顺序
- [ ] 能列举至少 5 种 Python 内置异常类型
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第五天：面向对象编程 — 类、继承、魔术方法**

第四天掌握了文件操作和异常处理后，第五天将进入面向对象编程。你有 C++ 的 OOP 基础，所以重点放在 Python 特有的 class 写法——`self` 参数、魔术方法（`__init__`、`__str__`、`__repr__`）、`@property` 装饰器、以及继承和多态在 Python 中的体现。

---

> ✨ **第四天的核心心法**：`with` 语句和 `try/except` 让脚本从"脆弱"变"健壮"。一个生产级别的测试框架，不是不会遇到错误，而是遇到了错误能优雅地处理、记录、继续执行。


# 第五天：面向对象编程 — 类、继承、魔术方法

> 📅 学习日期：第五天 | 🎯 模块：Python 基础 → 面向对象 | ⏱️ 建议学习时长：2-3 小时

---

## 前言：C++ 的 class 和 Python 的 class 有什么不同？

你有 C++ 基础，OOP 概念（封装、继承、多态）你已经懂了。所以今天的重点不是"什么是类"，而是 **Python 的 class 写法有什么特殊之处**。

| 维度 | C++ | Python |
|------|-----|--------|
| **构造函数** | `ClassName()` | `__init__(self)` |
| **析构函数** | `~ClassName()` | `__del__(self)`（少用，依赖 GC） |
| **this 指针** | `this->member`（可省略） | `self.member`（**必须显式**） |
| **访问控制** | `public`/`private`/`protected` | 全靠约定（`_` = 受保护，`__` = 名称改写） |
| **多继承** | 支持（有歧义风险） | 支持（MRO 线性化处理） |
| **运算符重载** | `operator+` | `__add__`（双下划线方法） |

---

## 一、类的定义与使用

### 1.1 第一个 Python 类

```python
class User:
    """用户类"""

    # 类属性（所有实例共享）
    platform = "Web"

    # 构造函数（初始化方法）
    def __init__(self, username, password, email=None):
        # self 必须显式写出！（类似 C++ 的 this）
        self.username = username     # 实例属性
        self.password = password
        self.email = email
        self.is_active = True        # 默认值

    # 实例方法
    def deactivate(self):
        """禁用用户"""
        self.is_active = False

    def __str__(self):
        """字符串表示（print 时调用）"""
        return f"User({self.username}, active={self.is_active})"

    def __repr__(self):
        """开发者表示（调试用）"""
        return f"User(username='{self.username}', email='{self.email}')"


# 使用
user1 = User("zane", "pass123", "zane@test.com")
user2 = User("alice", "pass456")          # email 使用默认值 None

print(user1)                  # User(zane, active=True)
print(user1.platform)         # "Web"（类属性）
user1.deactivate()
print(user1.is_active)        # False
```

> ⚠️ **Python OOP 铁律**：实例方法的第一个参数永远是 `self`，调用时不需要手动传，但**定义时必须写**。

### 1.2 类属性 vs 实例属性

```python
class Dog:
    species = "犬科"        # 类属性：所有实例共享

    def __init__(self, name):
        self.name = name    # 实例属性：每个实例独有

dog1 = Dog("旺财")
dog2 = Dog("来福")

print(dog1.species)         # "犬科"
print(dog2.species)         # "犬科"
Dog.species = "犬亚科"      # 修改类属性 → 所有实例都受影响
print(dog1.species)         # "犬亚科"

# ⚠️ 危险操作：通过实例修改类属性
dog1.species = "狼科"       # 这实际上是给 dog1 创建了一个同名的实例属性！
print(dog1.species)         # "狼科"  ← dog1 的实例属性
print(dog2.species)         # "犬亚科" ← 类属性没变
print(Dog.species)          # "犬亚科"
```

> 📌 **原则**：修改类属性永远通过 `类名.属性`，不要通过实例。

---

## 二、魔术方法（Magic Methods / Dunder Methods）

> ⚠️ **面试高频**：`__init__` vs `__new__`、`__str__` vs `__repr__`

### 2.1 常用魔术方法速查

| 魔术方法 | 触发场景 | 类比 C++ |
|----------|----------|----------|
| `__init__(self)` | 构造函数 | `ClassName()` 构造函数 |
| `__str__(self)` | `print(obj)` / `str(obj)` | 给用户看 |
| `__repr__(self)` | `repr(obj)` / 交互环境直接输入变量名 | 给开发者看 |
| `__len__(self)` | `len(obj)` | |
| `__eq__(self, other)` | `obj1 == obj2` | `operator==` |
| `__lt__(self, other)` | `obj1 < obj2` | `operator<` |
| `__getitem__(self, key)` | `obj[key]` | `operator[]` |
| `__contains__(self, item)` | `item in obj` | |
| `__call__(self)` | `obj()` | `operator()` 仿函数 |
| `__enter__/__exit__` | `with obj:` | 析构函数/RAII |

### 2.2 实战示例

```python
class TestCase:
    """测试用例类"""

    def __init__(self, case_id, title, priority="P2"):
        self.case_id = case_id
        self.title = title
        self.priority = priority
        self.result = None    # None=未执行, True=通过, False=失败

    def run(self, passed=True):
        """执行用例"""
        self.result = passed

    def __str__(self):
        """print(tc) — 给用户看"""
        status = "✅ 通过" if self.result else ("❌ 失败" if self.result is False else "⏸ 未执行")
        return f"[{self.priority}] {self.case_id}: {self.title} — {status}"

    def __repr__(self):
        """调试时输出"""
        return f"TestCase('{self.case_id}', '{self.title}', '{self.priority}')"

    def __eq__(self, other):
        """两个用例的 ID 相同就视为相同"""
        if not isinstance(other, TestCase):
            return False
        return self.case_id == other.case_id

    def __lt__(self, other):
        """按优先级排序：P0 < P1 < P2 < P3"""
        priority_order = {"P0": 0, "P1": 1, "P2": 2, "P3": 3}
        return priority_order[self.priority] < priority_order[other.priority]


# 使用
tc1 = TestCase("TC001", "登录成功", "P0")
tc2 = TestCase("TC002", "密码错误", "P1")
tc3 = TestCase("TC003", "用户名为空", "P2")

tc1.run(True)
tc2.run(False)

print(tc1)                      # [P0] TC001: 登录成功 — ✅ 通过
print(tc1 == tc2)               # False
print(sorted([tc3, tc1, tc2]))  # 按优先级排序
```

---

## 三、继承与多态

### 3.1 基本继承

```python
# 父类（基类）
class APITestCase:
    def __init__(self, method, url, expected_status=200):
        self.method = method
        self.url = url
        self.expected_status = expected_status
        self.response = None

    def send_request(self):
        """发送请求（子类可覆盖）"""
        print(f"发送 {self.method} 请求到 {self.url}")

    def validate(self):
        """验证响应（子类可覆盖）"""
        if self.response is None:
            raise ValueError("请先发送请求")
        # 默认验证逻辑
        return True


# 子类
class LoginTestCase(APITestCase):
    def __init__(self, username, password, expected_status=200):
        super().__init__("POST", "/api/login", expected_status)
        self.username = username
        self.password = password

    def send_request(self):
        """覆盖父类方法，发送登录请求"""
        print(f"发送 POST /api/login, body={self.username}:{self.password}")
        # 实际中这里会调用 requests.post(...)
        self.response = {"code": 0, "msg": "登录成功"}

    def validate(self):
        """覆盖父类方法，增加业务校验"""
        if not super().validate():    # 先调用父类的基本验证
            return False
        # 子类额外的业务验证
        return self.response.get("code") == 0


# 使用
tc = LoginTestCase("admin", "Pass123", 200)
tc.send_request()            # 发送 POST /api/login, body=admin:Pass123
print(tc.validate())         # True
```

### 3.2 `super()` 的用法

```python
class Base:
    def __init__(self, name):
        self.name = name
        print(f"Base.__init__: {name}")

class Child(Base):
    def __init__(self, name, age):
        super().__init__(name)    # Python 3 写法（推荐）
        # super(Child, self).__init__(name)  # Python 2 写法（兼容）
        self.age = age
        print(f"Child.__init__: {name}, {age}")

c = Child("Zane", 25)
# 输出：
# Base.__init__: Zane
# Child.__init__: Zane, 25
```

### 3.3 多继承与 MRO（Method Resolution Order）

```python
class A:
    def method(self):
        print("A")

class B(A):
    def method(self):
        print("B")

class C(A):
    def method(self):
        print("C")

class D(B, C):
    pass

d = D()
d.method()          # "B" — 按 MRO 顺序，B 在 C 前面
print(D.__mro__)    # 查看方法解析顺序
# (D, B, C, A, object)
```

> 📌 MRO 使用 **C3 线性化算法**，保证每个父类只被调用一次。基本上就是按继承声明顺序从左到右。

### 3.4 `isinstance()` 和 `issubclass()`

```python
tc = LoginTestCase("admin", "pass", 200)

print(isinstance(tc, LoginTestCase))    # True
print(isinstance(tc, APITestCase))      # True（子类实例也是父类实例）
print(issubclass(LoginTestCase, APITestCase))  # True
print(issubclass(LoginTestCase, object))       # True（所有类都继承自 object）
```

---

## 四、访问控制 — Python 的"约定优于强制"

```python
class Config:
    def __init__(self):
        self.host = "localhost"       # 公共属性
        self._port = 8080             # 约定：受保护的（别在外面用）
        self.__password = "secret"    # 名称改写：变成 _Config__password

    @property
    def port(self):
        """把方法变成属性调用"""
        return self._port

    @port.setter
    def port(self, value):
        if not 0 < value < 65536:
            raise ValueError("端口号非法")
        self._port = value

# 使用
cfg = Config()
print(cfg.host)             # "localhost" ✅
print(cfg._port)            # 8080（能访问但不建议）
# print(cfg.__password)     # AttributeError!
print(cfg._Config__password) # "secret"（名称改写后仍可访问，但说明你不该这样做）

print(cfg.port)             # 8080（像属性一样用，不需加括号）
cfg.port = 9090             # setter 自动触发校验
```

| Python 写法 | 含义 | C++ 类比 |
|-------------|------|----------|
| `self.name` | 公共 | `public` |
| `self._name` | 约定受保护（别在外面用） | `protected`（靠自觉） |
| `self.__name` | 名称改写为 `_ClassName__name` | `private`（软约束） |
| `@property` | getter | `getXxx()` |
| `@name.setter` | setter | `setXxx()` |

---

## 📝 今日练习

### 练习一：写一个 DataBaseConfig 类（15 分钟）

创建一个 `DataBaseConfig` 类，包含以下设计：

- 属性：host、port、user、password、database
- 使用 `@property` 对 port 做校验（1~65535）
- 实现 `__str__` 方法，输出格式：`"mysql://user@host:port/database"`（password 不显示）
- 实现 `__repr__` 方法

### 练习二：APITestCase 继承体系（15 分钟）

设计一个简单的测试用例继承体系：

1. `BaseTestCase`：包含 case_id、title、result 属性，`run()` 和 `__str__()` 方法
2. `APITestCase(BaseTestCase)`：增加 url、method、expected_status 属性，覆盖 `run()` 方法
3. `UITestCase(BaseTestCase)`：增加 page_url、element_xpath 属性，覆盖 `run()` 方法
4. 创建各子类的实例并调用 `run()` 验证

### 练习三：魔术方法实践（10 分钟）

为以下 `Order` 类实现魔术方法：

```python
class Order:
    def __init__(self, order_id, user, amount):
        self.order_id = order_id
        self.user = user
        self.amount = amount
```

实现：
1. `__eq__` — 两个订单 order_id 相同即相等
2. `__lt__` — 按金额从小到大排序
3. `__repr__` — 返回 `"Order(id, user, amount)"` 格式

---

## 📋 第五天自检清单

- [ ] Python 类的构造函数方法名是什么？（`__init__`）
- [ ] `self` 的作用是什么？为什么必须显式写出？
- [ ] 类属性和实例属性的区别是什么？修改时该注意什么？
- [ ] `__str__` 和 `__repr__` 的区别和使用场景
- [ ] 至少能说出 5 个魔术方法及其触发场景
- [ ] `super()` 的作用是什么？怎么写？
- [ ] Python 的"私有"属性是怎么实现的？（名称改写 `__name` → `_ClassName__name`）
- [ ] `@property` 装饰器的用途
- [ ] `isinstance()` 和 `issubclass()` 的区别
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第六天：pytest 测试框架 — 从 0 到 1 写测试用例**

面向对象学完后，第六天将进入测试工程师的核心技能——pytest。从基本的 `assert` 断言、`@pytest.fixture` 前置条件、`@pytest.mark.parametrize` 参数化测试、到测试报告生成。这是接口自动化框架的基石。

---

> ✨ **第五天的核心心法**：Python 的 OOP 比 C++ 更灵活（鸭子类型），也更依赖约定（命名规范）。掌握好魔术方法和 `@property`，能让你的类写得既简洁又强大。


# 第六天：pytest 测试框架 — 从 0 到 1 写测试用例

> 📅 学习日期：第六天 | 🎯 模块：Python 基础 → 测试框架 | ⏱️ 建议学习时长：3-4 小时

---

## 前言：为什么测试工程师必须掌握 pytest？

前面的 Python 基础让你"能用 Python 写脚本"。但要成为测试开发工程师，你必须掌握**测试框架**——它能帮你：

- 🏗️ **组织用例**：几百条用例不乱
- 📊 **生成报告**：自动统计通过率
- 🔄 **参数化**：一组数据跑 N 次，不用复制粘贴
- 🧩 **Fixture**：前置条件/后置清理 自动管理
- 🔌 **插件生态**：Allure 报告、并发执行、失败重试

> 🎯 **pytest 是 Python 测试生态的事实标准**。面试必问，工作必用。

---

## 一、pytest 快速入门

### 1.1 安装和第一个测试

```bash
pip install pytest
```

```python
# test_sample.py
def test_addition():
    """测试加法"""
    assert 1 + 1 == 2

def test_string():
    """测试字符串"""
    assert "hello".upper() == "HELLO"

def test_list():
    """测试列表"""
    assert len([1, 2, 3]) == 3
```

```bash
# 运行测试
pytest test_sample.py          # 运行指定文件
pytest test_sample.py -v       # verbose 详细输出
pytest test_sample.py -v -s    # -s 显示 print 输出
```

pytest 的**自动发现规则**：
- 文件名以 `test_` 开头或 `_test` 结尾
- 函数名以 `test_` 开头
- 类名以 `Test` 开头（不含 `__init__` 方法）

### 1.2 断言（assert）— 比 unittest 简洁 10 倍

```python
# pytest：直接用 assert（和写 Python 代码一样）
def test_assertions():
    assert 2 + 2 == 4
    assert "hello" in "hello world"
    assert [1, 2, 3] == [1, 2, 3]
    assert {"a": 1}.get("b") is None
    assert isinstance("hello", str)

# 对比 unittest 写法：
# import unittest
# class TestExample(unittest.TestCase):
#     def test_assertions(self):
#         self.assertEqual(2 + 2, 4)
#         self.assertIn("hello", "hello world")
#         self.assertEqual([1, 2, 3], [1, 2, 3])
#         self.assertIsNone({"a": 1}.get("b"))
#         self.assertIsInstance("hello", str)
```

> 📌 pytest 的 assert 失败时会自动展示**详细对比信息**——告诉你哪个值不对、差在哪里。

### 1.3 断言异常

```python
import pytest

def test_zero_division():
    """测试除零异常"""
    with pytest.raises(ZeroDivisionError):
        1 / 0

def test_value_error():
    """测试值错误，同时验证异常信息"""
    with pytest.raises(ValueError, match="invalid literal"):
        int("abc")
```

---

## 二、Fixture — 测试前置/后置条件

### 2.1 什么是 Fixture？

Fixture 就是**测试的环境准备和清理**。类比：
- 测试前创建测试用户 → 测试 → 测试后删除用户
- 测试前连接数据库 → 测试 → 测试后断开连接
- 测试前打开浏览器 → 测试 → 测试后关闭浏览器

### 2.2 基本 Fixture

```python
import pytest

# 定义 fixture
@pytest.fixture
def test_user():
    """创建一个测试用户，测试结束后清理"""
    user = {"id": 1, "username": "test_user", "email": "test@example.com"}
    print("\n🔧 前置: 创建测试用户")
    yield user                           # yield 之前 = 前置，yield 之后 = 后置
    print("\n🧹 后置: 删除测试用户")

# 使用 fixture
def test_user_creation(test_user):       # 参数名 = fixture 名
    assert test_user["username"] == "test_user"
    assert test_user["email"] == "test@example.com"

def test_user_id(test_user):
    assert test_user["id"] == 1
```

> 🎯 **yield 的精妙之处**：`yield user` 把 user 传给测试用例；测试执行完毕后，`yield` 后面的代码才执行（清理）。

### 2.3 Fixture 作用域

```python
@pytest.fixture(scope="function")   # 默认：每个测试函数调用一次
@pytest.fixture(scope="class")      # 每个测试类调用一次
@pytest.fixture(scope="module")     # 每个模块调用一次
@pytest.fixture(scope="session")    # 整个测试会话调用一次

# 示例：数据库连接用 session 级别（只连一次）
@pytest.fixture(scope="session")
def db_connection():
    print("连接数据库...")
    conn = {"connected": True}
    yield conn
    print("断开数据库连接...")
```

### 2.4 conftest.py — 共享 Fixture

```
project/
├── conftest.py          ← 这里的 fixture 对整个目录下的测试可见
├── test_api/
│   ├── conftest.py      ← 这里的 fixture 只对 test_api/ 下的测试可见
│   └── test_login.py
└── test_db/
    └── test_query.py
```

```python
# conftest.py — 自动被 pytest 加载，无需手动 import
import pytest

@pytest.fixture
def base_url():
    return "http://localhost:8080"

@pytest.fixture
def api_client(base_url):
    """返回一个配置好的测试客户端"""
    return {
        "base_url": base_url,
        "headers": {"Content-Type": "application/json"},
    }
```

### 2.5 Fixture 依赖链

```python
@pytest.fixture
def db_connection():
    print("1. 连接数据库")
    yield "db_conn"
    print("4. 断开数据库")

@pytest.fixture
def test_user(db_connection):   # 依赖 db_connection
    print("2. 在数据库中创建用户")
    yield {"id": 1, "name": "test"}
    print("3. 从数据库删除用户")

def test_something(test_user):   # 依赖 test_user → 自动依赖 db_connection
    assert test_user["name"] == "test"
```

---

## 三、参数化测试（⭐ 核心技能）

### 3.1 基本用法

```python
import pytest

# 一组数据跑多次
@pytest.mark.parametrize("a, b, expected", [
    (1, 2, 3),
    (0, 0, 0),
    (-1, 1, 0),
    (100, 200, 300),
])
def test_addition(a, b, expected):
    assert a + b == expected
```

运行结果：
```
test_param.py::test_addition[1-2-3] PASSED
test_param.py::test_addition[0-0-0] PASSED
test_param.py::test_addition[-1-1-0] PASSED
test_param.py::test_addition[100-200-300] PASSED
```

### 3.2 接口测试参数化（实战模式）

```python
import pytest

# 登录接口测试 — 数据驱动
@pytest.mark.parametrize("username, password, expected_code, expected_msg", [
    # 正向用例
    ("admin", "Pass1234", 200, "登录成功"),
    # 反向用例
    ("", "Pass1234", 400, "用户名不能为空"),
    ("admin", "", 400, "密码不能为空"),
    ("not_exist", "Pass1234", 401, "用户不存在"),
    ("admin", "wrong_pass", 401, "密码错误"),
    ("admin", "Pass1234" * 100, 400, "密码过长"),
])
def test_login(api_client, username, password, expected_code, expected_msg):
    """登录接口参数化测试"""
    response = api_client["post"]("/api/login", {
        "username": username,
        "password": password,
    })
    assert response["status_code"] == expected_code
    assert response["body"]["msg"] == expected_msg
```

### 3.3 堆叠参数化（多维度组合）

```python
@pytest.mark.parametrize("method", ["POST", "PUT"])
@pytest.mark.parametrize("url", ["/api/user", "/api/order"])
@pytest.mark.parametrize("auth", [True, False])
def test_api_combination(method, url, auth):
    """method × url × auth = 2×2×2 = 8 条用例"""
    print(f"测试: {method} {url}, auth={auth}")
```

---

## 四、pytest 常用命令行选项

```bash
pytest                              # 运行所有测试
pytest test_file.py                 # 运行指定文件
pytest test_file.py::test_func      # 运行指定函数
pytest -v                           # verbose 详细输出
pytest -s                           # 显示 print 输出
pytest -x                           # 遇到第一个失败就停止
pytest --maxfail=3                   # 遇到第 3 个失败停止
pytest -k "login"                   # 只运行名称包含 "login" 的用例
pytest -m "smoke"                   # 只运行标记为 smoke 的用例
pytest --lf                         # 只运行上次失败的用例
pytest --ff                         # 先运行上次失败的，再运行其他的
pytest -n auto                      # 并行执行（需安装 pytest-xdist）
pytest --durations=5                # 显示最慢的 5 个用例
pytest --tb=short                   # 缩短 traceback 输出
```

---

## 五、标记（Mark）— 分类和组织用例

### 5.1 内置标记

```python
import pytest

@pytest.mark.skip(reason="暂时跳过，等待 Bug #123 修复")
def test_unstable():
    pass

@pytest.mark.skipif(sys.platform == "win32", reason="Windows 下不适用")
def test_linux_only():
    pass

@pytest.mark.xfail(reason="已知 Bug，预期失败")
def test_known_bug():
    assert 1 == 2
```

### 5.2 自定义标记

```ini
# pytest.ini — 注册自定义标记
[pytest]
markers =
    smoke: 冒烟测试用例
    regression: 回归测试用例
    slow: 耗时较长的测试
    api: 接口测试用例
```

```python
@pytest.mark.smoke
def test_login_success():
    """冒烟测试：核心流程"""
    pass

@pytest.mark.slow
@pytest.mark.regression
def test_big_data_export():
    """慢 + 回归测试"""
    pass

# 运行：
# pytest -m "smoke"              # 只跑冒烟
# pytest -m "not slow"           # 跳过慢测试
# pytest -m "smoke and api"      # 同时满足两个标记
```

---

## 六、测试报告

### 6.1 pytest-html

```bash
pip install pytest-html

pytest --html=report.html --self-contained-html
```

### 6.2 Allure 报告（⭐ 企业级推荐）

```bash
pip install allure-pytest

pytest --alluredir=./allure-results
allure serve ./allure-results      # 启动查看报告
```

---

## 📝 今日练习

### 练习一：写一个计算器的测试（15 分钟）

写一个 `calculator.py`，包含 `add`、`subtract`、`multiply`、`divide` 四个函数。然后用 pytest 写 `test_calculator.py`：
- 每个函数至少 3 条用例
- 使用参数化
- `divide` 函数测试除以零的异常

### 练习二：Fixture 练习（15 分钟）

写一个用户管理的测试：
1. 用 fixture 创建测试用户列表
2. 用 yield 在测试结束后清理
3. 测试用户搜索功能（按用户名查找）
4. 测试用户删除功能

### 练习三：接口测试参数化（15 分钟）

用 pytest.mark.parametrize 设计一个完整的登录接口测试用例集：
- 至少 8 条用例（1 条正向 + 7 条反向）
- 覆盖：空用户名、空密码、用户不存在、密码错误、用户名过长、密码过短、SQL 注入
- 每条用例包含：用例描述、输入数据、预期状态码、预期错误信息

---

## 📋 第六天自检清单

- [ ] pytest 安装成功了吗？能运行第一个测试吗？
- [ ] pytest 的自动发现规则是什么（文件名、函数名、类名）？
- [ ] `assert` 和 unittest 的 `self.assertEqual` 哪个更简洁？
- [ ] Fixture 的 `yield` 前后的代码分别什么时候执行？
- [ ] Fixture 的四种作用域（function/class/module/session）的区别
- [ ] `conftest.py` 的作用是什么？
- [ ] `@pytest.mark.parametrize` 的语法和用途
- [ ] 知道至少 5 个 pytest 命令行选项
- [ ] 自定义标记（`@pytest.mark.smoke`）怎么注册和使用？
- [ ] 完成三道课后练习

---

## 🔜 下一天预告

**第七天：requests 库 + 接口自动化实战**

pytest 提供了测试框架，requests 提供了 HTTP 请求能力——两者结合就是接口自动化测试。第七天将学习 requests 库的核心用法，然后结合 pytest 搭建一个最小可用的接口自动化框架。

---

> ✨ **第六天的核心心法**：pytest 是测试开发工程师的"瑞士军刀"。参数化让你用数据驱动测试，Fixture 让你优雅管理环境，标记让你灵活筛选用例。掌握了 pytest，你就掌握了自动化测试的"操作系统"。


# 第七天：requests 库 + 接口自动化实战

> 📅 学习日期：第七天 | 🎯 模块：Python 基础 → 接口自动化 | ⏱️ 建议学习时长：3-4 小时

---

## 前言：从"手工发请求"到"自动化测试"

前面六天学完了 Python 基础 + pytest 测试框架。今天把它们结合起来：

> **Python 基础（变量/函数/OOP）+ pytest（用例组织/参数化/Fixture）+ requests（发 HTTP 请求）= 接口自动化测试框架**

---

## 一、requests 库核心用法

### 1.1 安装和第一个请求

```bash
pip install requests
```

```python
import requests

# GET 请求
response = requests.get("https://httpbin.org/get")
print(response.status_code)      # 200
print(response.text)             # 响应体（字符串）
print(response.json())           # 响应体（JSON → dict）

# 带参数的 GET
response = requests.get("https://httpbin.org/get", params={"key": "value", "page": 1})
print(response.url)   # https://httpbin.org/get?key=value&page=1
```

### 1.2 Response 对象详解

```python
response = requests.get("https://httpbin.org/get")

# 状态相关
response.status_code       # 200
response.ok                # True（status_code < 400）
response.reason            # "OK"

# 响应头
response.headers           # dict-like
response.headers["Content-Type"]

# 响应体
response.text              # str（自动解码）
response.content           # bytes（原始字节流）
response.json()            # dict（解析 JSON）

# 其他
response.elapsed           # timedelta（响应耗时）
response.url               # 最终 URL（跟随重定向后的）
response.request.headers   # 实际发送的请求头（调试用）
```

### 1.3 各种 HTTP 方法

```python
# POST — JSON 格式（⭐ 最常用）
response = requests.post(
    "https://httpbin.org/post",
    json={"username": "admin", "password": "Pass1234"},
    headers={"Authorization": "Bearer token123"},
)

# POST — Form 表单格式
response = requests.post(
    "https://httpbin.org/post",
    data={"username": "admin", "password": "Pass1234"},
)

# PUT / DELETE / PATCH
requests.put("https://httpbin.org/put", json={"key": "updated_value"})
requests.delete("https://httpbin.org/delete")
requests.patch("https://httpbin.org/patch", json={"key": "patched_value"})

# HEAD（只获取响应头，不要响应体）
requests.head("https://httpbin.org/get")
```

### 1.4 请求头和认证

```python
# 自定义 Header
headers = {
    "Content-Type": "application/json",
    "Authorization": "Bearer eyJhbGciOi...",
    "User-Agent": "TestAgent/1.0",
}
response = requests.get("https://httpbin.org/headers", headers=headers)

# Basic Auth
from requests.auth import HTTPBasicAuth
requests.get("https://httpbin.org/basic-auth/user/pass", auth=HTTPBasicAuth("user", "pass"))
# 或简写
requests.get("https://httpbin.org/basic-auth/user/pass", auth=("user", "pass"))
```

### 1.5 Session — 保持 Cookie 和连接

```python
# 使用 Session 保持登录状态（⭐ 接口测试必用）
session = requests.Session()

# 登录
login_resp = session.post(
    "https://httpbin.org/post",
    json={"username": "admin", "password": "pass"},
)
# Session 自动保存 Cookie

# 后续请求自动携带 Cookie
info_resp = session.get("https://httpbin.org/get")
```

> 📌 为什么用 Session？① 自动管理 Cookie；② 连接复用（底层 TCP 连接不销毁）；③ 统一设置 headers（不用每次传 Authorization）。

### 1.6 超时和重试

```python
# 设置超时（⭐ 生产代码必须设置）
try:
    response = requests.get("https://httpbin.org/delay/5", timeout=3)
except requests.Timeout:
    print("请求超时！")

# timeout 可以分别设置连接超时和读取超时
response = requests.get("https://httpbin.org/get", timeout=(3, 10))
#                                                           连接超时 3s, 读取超时 10s
```

### 1.7 响应内容校验

```python
response = requests.post("https://httpbin.org/post", json={"name": "Zane"})
data = response.json()

# 状态码断言
assert response.status_code == 200

# JSON 字段断言
assert data["json"]["name"] == "Zane"

# 响应头断言
assert response.headers["Content-Type"] == "application/json"

# 响应时间断言
assert response.elapsed.total_seconds() < 2.0
```

---

## 二、封装一个测试专用的 HTTP 客户端

```python
import requests
import json
import logging

class APIClient:
    """接口测试专用 HTTP 客户端"""

    def __init__(self, base_url, token=None, timeout=10):
        self.base_url = base_url.rstrip("/")
        self.timeout = timeout
        self.session = requests.Session()

        # 统一设置默认 Header
        self.session.headers.update({
            "Content-Type": "application/json",
            "User-Agent": "TestClient/1.0",
        })

        if token:
            self.session.headers["Authorization"] = f"Bearer {token}"

    def _full_url(self, path):
        """拼接完整 URL"""
        return f"{self.base_url}{path}"

    def _log_response(self, response):
        """记录响应信息（调试用）"""
        logging.info(f"{response.request.method} {response.url} → {response.status_code}")
        if not response.ok:
            logging.error(f"响应体: {response.text[:500]}")

    def get(self, path, **kwargs):
        """GET 请求"""
        kwargs.setdefault("timeout", self.timeout)
        response = self.session.get(self._full_url(path), **kwargs)
        self._log_response(response)
        return response

    def post(self, path, data=None, **kwargs):
        """POST 请求（自动转 JSON）"""
        kwargs.setdefault("timeout", self.timeout)
        if data and "json" not in kwargs:
            kwargs["json"] = data
        response = self.session.post(self._full_url(path), **kwargs)
        self._log_response(response)
        return response

    def put(self, path, data=None, **kwargs):
        """PUT 请求"""
        kwargs.setdefault("timeout", self.timeout)
        if data and "json" not in kwargs:
            kwargs["json"] = data
        response = self.session.put(self._full_url(path), **kwargs)
        self._log_response(response)
        return response

    def delete(self, path, **kwargs):
        """DELETE 请求"""
        kwargs.setdefault("timeout", self.timeout)
        response = self.session.delete(self._full_url(path), **kwargs)
        self._log_response(response)
        return response

    def login(self, login_path, username, password):
        """登录并自动保存 Token"""
        response = self.post(login_path, {"username": username, "password": password})
        if response.status_code == 200:
            token = response.json().get("token")
            if token:
                self.session.headers["Authorization"] = f"Bearer {token}"
        return response
```

---

## 三、pytest + requests = 最小接口自动化框架

### 3.1 项目结构

```
api_test_framework/
├── conftest.py           # Fixture：api_client
├── pytest.ini            # 配置和标记注册
├── test_cases/
│   ├── test_login.py     # 登录模块测试
│   └── test_user.py      # 用户模块测试
├── test_data/
│   └── login_data.json   # 测试数据文件
└── reports/              # 测试报告输出目录
```

### 3.2 conftest.py

```python
import pytest
import sys
import os

# 添加项目根目录到 Python path
sys.path.insert(0, os.path.dirname(__file__))

# 假设上面封装的 APIClient 在 client.py 中
from client import APIClient

@pytest.fixture(scope="session")
def api_client():
    """创建 API 客户端（整个测试会话共用）"""
    client = APIClient(
        base_url="https://httpbin.org",  # 测试环境
        timeout=10,
    )
    # 登录获取 Token
    # client.login("/api/login", "admin", "Pass1234")
    return client
```

### 3.3 test_login.py（完整示例）

```python
import pytest
import json
import os

# 从 JSON 文件加载测试数据
def load_login_test_data():
    filepath = os.path.join(os.path.dirname(__file__), "../test_data/login_data.json")
    with open(filepath, "r", encoding="utf-8") as f:
        return json.load(f)

class TestLogin:
    """登录接口测试套件"""

    @pytest.mark.smoke
    def test_login_success(self, api_client):
        """冒烟测试：正确用户名+密码 → 200"""
        response = api_client.post("/post", {
            "username": "admin",
            "password": "Pass1234",
        })
        assert response.status_code == 200

    @pytest.mark.parametrize("case", load_login_test_data())
    def test_login_parametrized(self, api_client, case):
        """参数化测试：从 JSON 文件读取测试数据"""
        response = api_client.post("/post", {
            "username": case["username"],
            "password": case["password"],
        })

        # 断言状态码
        assert response.status_code == case["expected_status"], \
            f"状态码不匹配: 期望 {case['expected_status']}, 实际 {response.status_code}"

    @pytest.mark.regression
    def test_login_response_format(self, api_client):
        """验证响应体格式"""
        response = api_client.post("/post", {
            "username": "admin",
            "password": "Pass1234",
        })
        data = response.json()
        assert "json" in data
        assert data["json"]["username"] == "admin"

    @pytest.mark.parametrize("field", ["username", "password"])
    def test_login_missing_field(self, api_client, field):
        """缺少必填字段 → 400"""
        data = {"username": "admin", "password": "pass"}
        del data[field]
        response = api_client.post("/post", data)
        # httpbin 不校验字段，实际项目中应 assert response.status_code == 400
        assert response.status_code == 200  # httpbin 接受任何 POST
```

### 3.4 测试数据 JSON 文件

```json
[
  {
    "case_id": "TC-LOGIN-001",
    "title": "正确的用户名和密码",
    "username": "admin",
    "password": "Pass1234",
    "expected_status": 200
  },
  {
    "case_id": "TC-LOGIN-002",
    "title": "用户名为空",
    "username": "",
    "password": "Pass1234",
    "expected_status": 400
  },
  {
    "case_id": "TC-LOGIN-003",
    "title": "密码为空",
    "username": "admin",
    "password": "",
    "expected_status": 400
  },
  {
    "case_id": "TC-LOGIN-004",
    "title": "用户不存在",
    "username": "not_exist_user",
    "password": "Pass1234",
    "expected_status": 401
  },
  {
    "case_id": "TC-LOGIN-005",
    "title": "SQL 注入测试",
    "username": "' OR '1'='1",
    "password": "' OR '1'='1",
    "expected_status": 400
  }
]
```

---

## 四、常用 Python 测试工具库

| 库 | 用途 | 安装 |
|----|------|------|
| **requests** | HTTP 请求 | `pip install requests` |
| **pytest** | 测试框架 | `pip install pytest` |
| **pytest-html** | HTML 测试报告 | `pip install pytest-html` |
| **allure-pytest** | Allure 测试报告 | `pip install allure-pytest` |
| **faker** | 随机测试数据生成 | `pip install faker` |
| **PyMySQL** | MySQL 数据库操作 | `pip install pymysql` |
| **openpyxl** | Excel 读写 | `pip install openpyxl` |
| **PyYAML** | YAML 配置文件 | `pip install pyyaml` |
| **loguru** | 更好的日志 | `pip install loguru` |
| **pytest-xdist** | 并行执行 | `pip install pytest-xdist` |

---

## 📝 今日练习

### 练习一：requests 基础（15 分钟）

1. 用 requests 发一个 GET 请求到 `https://httpbin.org/get`
2. 用 requests 发一个 POST 请求到 `https://httpbin.org/post`，携带 JSON 数据 `{"name": "你的名字"}`
3. 分别打印两个响应的：状态码、Content-Type 头、JSON 响应体

### 练习二：封装 HTTP 客户端（15 分钟）

写一个 `HttpClient` 类，封装 GET 和 POST 方法：
- 初始化时传入 base_url
- 自动添加 Content-Type: application/json 头
- 自动打印请求日志（URL、方法、状态码）
- 如果响应状态码不是 2xx，打印错误信息

### 练习三：完整的接口测试用例（20 分钟）

使用 pytest + requests：
1. 用 conftest.py 创建 `api_client` fixture
2. 写 3 个测试函数测试 `https://httpbin.org` 的不同端点
3. 使用参数化，用 5 组不同的 JSON 数据测试 POST 请求
4. 添加异常处理：如果请求超时，用例标记为失败而不崩溃

---

## 📋 第七天自检清单

- [ ] `requests.get()` 和 `requests.post()` 的 `params`、`json`、`data` 参数的区别
- [ ] `response.json()`、`response.text`、`response.content` 的区别
- [ ] `requests.Session()` 的作用是什么？为什么接口测试推荐用它？
- [ ] `timeout` 参数为什么必须设置？
- [ ] 如何获取响应的耗时（`response.elapsed`）？
- [ ] 能用 pytest + requests 写一个完整的接口测试用例吗？
- [ ] 能用 `@pytest.mark.parametrize` + JSON 文件做数据驱动测试吗？
- [ ] 知道至少 5 个 Python 测试相关的第三方库
- [ ] 完成三道课后练习

---

## 🎓 Python 模块总结

七天学习覆盖了 Python 基础到接口自动化的完整链路：

```
Day 1-2: 基础语法 + 数据结构      → 能写 Python 脚本
Day 3:   函数进阶（lambda/生成器） → 写出 Pythonic 的代码
Day 4:   文件操作 + 异常处理      → 脚本更健壮
Day 5:   面向对象编程             → 构建可复用的类
Day 6:   pytest 测试框架          → 组织和管理测试用例
Day 7:   requests + 接口自动化    → 搭建测试框架
```

> 🚀 **下一步**：Python 模块结束后，建议进入**模块五（接口测试 + JMeter）**或**模块七（接口自动化框架深入）**，进一步强化接口测试能力。准备好了就告诉我！

---

> ✨ **第七天的核心心法**：Python 基础 × pytest × requests = 接口自动化测试。三者不是孤立的——Python 是语言，pytest 是框架，requests 是工具。把它们练到"肌肉记忆"的程度，面试中的手写代码题和框架设计题就都难不倒你了。
