# GDB Pretty Printers

<a href="https://github.com/randomseed713/GDB-Pretty-Printers/stargazers"><img src="https://img.shields.io/github/stars/randomseed713/GDB-Pretty-Printers?style=flat"></a>
<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
<img src="https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white">
<img src="https://img.shields.io/badge/-Markdown-000000?logo=markdown&logoColor=white">

[English Documentation](README.md)

用于 GDB 调试器的 pretty printers，支持以下库：

- **nlohmann/json** — JSON 对象美化显示
- **Abseil** — `flat_hash_map` / `flat_hash_set` / `node_hash_map` / `node_hash_set` 容器美化显示

---

## GDB Pretty Printer for nlohmann/json

用于在调试时美化显示 nlohmann/json 库的 JSON 对象。

### 功能

在 GDB 调试 C++ 程序时，默认情况下 `nlohmann::json` 对象会显示为复杂的内部结构。使用这个 pretty printer 后，可以直接看到 JSON 的实际值，使调试更加直观。

### 使用方法

**方法 1 — 在 GDB 会话中手动加载**

```bash
(gdb) source /path/to/nlohmann-json.py
```

**方法 2 — 通过 `.gdbinit` 自动加载**

在项目根目录或用户主目录的 `.gdbinit` 文件中添加：

```python
source /path/to/nlohmann-json.py
```

**方法 3 — VSCode 配置**

在 `.vscode/launch.json` 的 `setupCommands` 中添加：

```json
{
    "description": "Load nlohmann/json pretty printer",
    "text": "-interpreter-exec console \"source /path/to/nlohmann-json.py\"",
    "ignoreFailures": true
}
```

### 示例

调试前（未使用 pretty printer）：

```
(gdb) print json_obj
$1 = {m_data = {m_type = nlohmann::detail::value_t::object, m_value = {...}}}
```

调试后（使用 pretty printer）：

```
(gdb) print json_obj
$1 = std::map with 3 elements = {
  ["name"] = "example",
  ["value"] = 42,
  ["active"] = true
}
```

---

## GDB Pretty Printer for Abseil 容器

用于在调试时美化显示 Abseil 容器的内容。

### 支持的容器类型

| 容器类型 |
|---------|
| `absl::flat_hash_map` |
| `absl::flat_hash_set` |
| `absl::node_hash_map` |
| `absl::node_hash_set` |

### 使用方法

**方法 1 — 在 GDB 会话中手动加载**

```bash
(gdb) source /path/to/absl.py
```

**方法 2 — 通过 `.gdbinit` 自动加载**

在项目根目录或用户主目录的 `.gdbinit` 文件中添加：

```python
source /path/to/absl.py
```

**方法 3 — VSCode 配置**

在 `.vscode/launch.json` 的 `setupCommands` 中添加：

```json
{
    "description": "Load Abseil pretty printer",
    "text": "-interpreter-exec console \"source /path/to/absl.py\"",
    "ignoreFailures": true
}
```

### 配置 Abseil 版本

编辑 `absl.py` 文件，修改以下变量以匹配你的 Abseil 版本：

```python
# 修改为与你的 Abseil 版本一致
ABSL_OPTION_INLINE_NAMESPACE_NAME = "lts_20240116"
```

查看 Abseil 版本的方法：
- 查看 `absl/base/options.h` 中的 `ABSL_OPTION_INLINE_NAMESPACE_NAME` 定义
- 或者查看编译错误信息中的内联命名空间名称

### 示例

调试前（未使用 pretty printer）：

```
(gdb) print flat_map
$1 = {settings_ = {...}, ...}
```

调试后（使用 pretty printer）：

```
(gdb) print flat_map
$1 = absl::flat_hash_map<std::string, int> with 3 elems  = {
  ["banana"] = 2,
  ["apple"] = 1,
  ["cherry"] = 3
}

(gdb) print flat_set
$2 = absl::flat_hash_set<int> with 3 elems  = {20, 10, 30}
```
