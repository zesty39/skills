---
name: shell-script-conventions
description: 规范 Bash 脚本的结构、变量作用域与命名、帮助信息、日志函数、参数解析与验证。用户创建、重构、审查或修复 Bash 脚本，尤其要求标准脚本布局、print_usage、readonly 变量或 info/warn/error 日志时使用；不适用于要求纯 POSIX sh 兼容的脚本。
---

# Shell Script Conventions

创建或修改 Bash 脚本时，先读取项目和全局规范，再遵循以下结构与检查流程。根据脚本的实际职责设计参数和流程，不要把某个脚本的专用参数模式强加到其他脚本。纯 POSIX `sh` 脚本继续遵循项目自身的兼容性规范。

## Structure

使用清晰的三段式布局：

1. `Variables`：设置脚本常量、默认值和运行时变量。
2. `Functions`：定义脚本实际使用的函数。
3. `Main`：在文件末尾直接执行主流程；不要为了包装主流程而额外创建 `main` 函数。

在变量赋值完成后留一个空行，再声明不会改变的变量。每个只读变量单独声明：

```bash
SCRIPT_DIR=...
SCRIPT_NAME=...
RUNTIME=

readonly SCRIPT_DIR
readonly SCRIPT_NAME
```

只将运行期间确实不会改变的变量声明为 `readonly`。需要被参数或函数更新的变量保持可修改。

按变量作用域统一命名：

- 函数外的全局变量使用全大写的 `UPPER_SNAKE_CASE`，例如 `SCRIPT_DIR`、`RUN_DIR`。
- 函数内创建的临时变量必须使用 `local` 声明，并使用全小写的 `snake_case`，例如 `local -a tty_args=()`；函数可以读取已经明确声明的全局常量。
- 函数需要更新全局状态时，直接使用已声明的全局变量，并让这一副作用在函数职责中保持明确。

脚本通常应以严格模式开始：

```bash
#!/usr/bin/env bash
set -euo pipefail
```

## Header and `print_usage`

每个脚本必须有 `print_usage` 函数，并且帮助内容必须来自脚本头部注释，而不是在函数中重复维护一份文本。使用明确的注释标记包围帮助区间：

```bash
# --- HELP BEGIN ---
# Usage:
#   ./script.sh [OPTIONS]
#
# Options:
#   -h, --help  Show this help message.
# --- HELP END ---
```

需要读取自身内容时，定义 `SCRIPT_DIR` 和 `SCRIPT_NAME`，让 `print_usage` 无需参数即可定位脚本：

```bash
print_usage() {
  sed -n \
    -e '/^# --- HELP BEGIN ---$/,/^# --- HELP END ---$/!d' \
    -e '/^# --- HELP BEGIN ---$/d' \
    -e '/^# --- HELP END ---$/d' \
    -e 's/^# //' \
    -e 's/^#$//' \
    -e 'p' \
    "$SCRIPT_DIR/$SCRIPT_NAME"
}
```

让帮助文本与脚本实际参数保持同步。至少验证 `--help` 输出、缺少必需参数和未知选项等路径。

## Logging

日志函数按实际使用声明，不要声明未使用的函数。日志函数名称和输出格式固定如下：

```bash
info() {
  printf '[INFO]: %s\n' "$*"
}

warn() {
  printf '[WARN]: %s\n' "$*" >&2
}

error() {
  printf '[ERROR]: %s\n' "$*" >&2
  exit 2
}
```

如果脚本只使用 `error`，只声明 `error`；如果还使用 `info` 或 `warn`，再声明对应函数。`error` 用于替代 `die` 一类的终止函数：输出错误后退出，不要同时保留功能重复的 `die`。

## Parameters and flow

根据具体脚本职责选择标准的短参数和长参数形式，例如 `-h`/`--help` 以及带值选项的 `--option VALUE`、`--option=VALUE`。参数解析应集中放在 `Main`，解析失败时使用 `error` 并给出简洁的帮助提示。

函数应保持单一职责，参数校验、路径解析、运行时选择和外部命令执行分别封装，避免把无关逻辑塞进参数解析循环。只有主流程负责按顺序解析参数、调用函数并结束脚本。

## Validation

修改后至少执行：

```bash
bash -n path/to/script.sh
```

如果环境安装了 ShellCheck，再执行 ShellCheck。根据脚本职责补充 `--help`、参数错误、路径错误和关键命令参数的行为测试；检查脚本是否意外包含密码、令牌、私钥或其他敏感值。
