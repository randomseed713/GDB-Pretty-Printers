# GDB Pretty Printers

<a href="https://github.com/randomseed713/GDB-Pretty-Printers/stargazers"><img src="https://img.shields.io/github/stars/randomseed713/GDB-Pretty-Printers?style=flat"></a>
<a href="https://opensource.org/licenses/MIT"><img src="https://img.shields.io/badge/license-MIT-blue.svg"></a>
<img src="https://img.shields.io/badge/-Python-3776AB?logo=python&logoColor=white">
<img src="https://img.shields.io/badge/-Markdown-000000?logo=markdown&logoColor=white">

[中文文档](README.zh-CN.md)

---

GDB pretty printers for the following libraries:

- **nlohmann/json** — Pretty-print JSON objects during debugging
- **Abseil** — Pretty-print `flat_hash_map` / `flat_hash_set` / `node_hash_map` / `node_hash_set` containers

---

### GDB Pretty Printer for nlohmann/json

Displays `nlohmann::json` objects in a human-readable form while debugging C++ programs.

#### Features

By default, a `nlohmann::json` object appears as a deeply nested internal structure in GDB. With this pretty printer, you see the actual JSON value directly, making debugging much more intuitive.

#### Usage

**Method 1 — Load manually in a GDB session**

```bash
(gdb) source /path/to/nlohmann-json.py
```

**Method 2 — Auto-load via `.gdbinit`**

Add the following line to the `.gdbinit` in your project root or home directory:

```python
source /path/to/nlohmann-json.py
```

**Method 3 — VSCode configuration**

Add the following entry to `setupCommands` in `.vscode/launch.json`:

```json
{
    "description": "Load nlohmann/json pretty printer",
    "text": "-interpreter-exec console \"source /path/to/nlohmann-json.py\"",
    "ignoreFailures": true
}
```

#### Example

*Before (without pretty printer)*

```
(gdb) print json_obj
$1 = {m_data = {m_type = nlohmann::detail::value_t::object, m_value = {...}}}
```

*After (with pretty printer)*

```
(gdb) print json_obj
$1 = std::map with 3 elements = {
  ["name"] = "example",
  ["value"] = 42,
  ["active"] = true
}
```

---

### GDB Pretty Printer for Abseil Containers

Displays the contents of Abseil hash containers in a human-readable form.

#### Supported container types

| Container |
|-----------|
| `absl::flat_hash_map` |
| `absl::flat_hash_set` |
| `absl::node_hash_map` |
| `absl::node_hash_set` |

#### Usage

**Method 1 — Load manually in a GDB session**

```bash
(gdb) source /path/to/absl.py
```

**Method 2 — Auto-load via `.gdbinit`**

Add the following line to the `.gdbinit` in your project root or home directory:

```python
source /path/to/absl.py
```

**Method 3 — VSCode configuration**

Add the following entry to `setupCommands` in `.vscode/launch.json`:

```json
{
    "description": "Load Abseil pretty printer",
    "text": "-interpreter-exec console \"source /path/to/absl.py\"",
    "ignoreFailures": true
}
```

#### Configuring the Abseil version

Edit `absl.py` and update the following variable to match your Abseil version:

```python
# Change this to match your Abseil version
ABSL_OPTION_INLINE_NAMESPACE_NAME = "lts_20240116"
```

How to find your Abseil version:
- Check the `ABSL_OPTION_INLINE_NAMESPACE_NAME` definition in `absl/base/options.h`
- Or look at the inline namespace name in compiler error messages

#### Example

*Before (without pretty printer)*

```
(gdb) print flat_map
$1 = {settings_ = {...}, ...}
```

*After (with pretty printer)*

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
